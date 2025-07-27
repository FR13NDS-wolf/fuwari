---
title: TelegramAPI反向代理
published: 2025-01-23
description: '部署在workers上,主要实现webhooks'
image: ''
tags: [Telegram, Workers, API, Proxy]
category: '教程'
draft: false 
lang: ''
---

我使用了2个workers来实现, 因为在不同的软件里面还不能直接调用

## Telegram-DDNS

这个我是用来搭建DDNS-GO上的 webhook

```js
const whitelist = ["/bot6112359xxx:xxx"];
//示例const whitelist = ["/bot123456:"];
const tg_host = "api.telegram.org";

addEventListener('fetch', event => {
    event.respondWith(handleRequest(event.request))
})

function validate(path) {
    for (var i = 0; i < whitelist.length; i++) {
        if (path.startsWith(whitelist[i]))
            return true;
    }
    return false;
}

async function handleRequest(request) {
    var u = new URL(request.url);
    u.host = tg_host;
    if (!validate(u.pathname))
        return new Response('Unauthorized', {
            status: 403
        });
    var req = new Request(u, {
        method: request.method,
        headers: request.headers,
        body: request.body
    });
    const result = await fetch(req);
    return result;
}

```

### DDNS-GO配置

```shell
https://webhook-tg.loophy.top/bot63xxx/sendMessage
{
    "chat_id": "-xxx",
    "text": "主人！您的 IP 有变化\n\nIPv6: #{ipv6Result}\nIP: #{ipv6Addr}\nDomain: xxx.xxx.com"

}
```

![image.png](https://teleimg.loophy.top/file/1753607403016_image.png)

## Telegram-SMS

这一个用做短信转发 [Smsforwarder](https://github.com/pppscn/SmsForwarder/)

```js
/**
 * Helper functions to check if the request uses
 * corresponding method.
 *
 */
const Method = (method) => (req) => req.method.toLowerCase() === method.toLowerCase();
const Get = Method('get');
const Post = Method('post');

const Path = (regExp) => (req) => {
	const url = new URL(req.url);
	const path = url.pathname;
	return path.match(regExp) && path.match(regExp)[0] === path;
};

/*
 * The regex to get the bot_token and api_method from request URL
 * as the first and second backreference respectively.
 */
const URL_PATH_REGEX = /^\/bot(?<bot_token>[^/]+)\/(?<api_method>[a-z]+)/i;

/**
 * Router handles the logic of what handler is matched given conditions
 * for each request
 */
class Router {
	constructor() {
		this.routes = [];
	}

	handle(conditions, handler) {
		this.routes.push({
			conditions,
			handler,
		});
		return this;
	}

	get(url, handler) {
		return this.handle([Get, Path(url)], handler);
	}

	post(url, handler) {
		return this.handle([Post, Path(url)], handler);
	}

	all(handler) {
		return this.handler([], handler);
	}

	route(req) {
		const route = this.resolve(req);

		if (route) {
			return route.handler(req);
		}

		const description = 'No matching route found';
		const error_code = 404;

		return new Response(
			JSON.stringify({
				ok: false,
				error_code,
				description,
			}),
			{
				status: error_code,
				statusText: description,
				headers: {
					'content-type': 'application/json',
				},
			}
		);
	}

	/**
	 * It returns the matching route that returns true
	 * for all the conditions if any.
	 */
	resolve(req) {
		return this.routes.find((r) => {
			if (!r.conditions || (Array.isArray(r) && !r.conditions.length)) {
				return true;
			}

			if (typeof r.conditions === 'function') {
				return r.conditions(req);
			}

			return r.conditions.every((c) => c(req));
		});
	}
}

/**
 * Sends a POST request with JSON data to Telegram Bot API
 * and reads in the response body.
 * @param {Request} request the incoming request
 */
async function handler(request) {
	// Extract the URl method from the request.
	const { url, ..._request } = request;

	const { pathname: path, search } = new URL(url);

	// Leave the first match as we are interested only in backreferences.
	const { bot_token, api_method } = path.match(URL_PATH_REGEX).groups;

	// Build the URL
	const api_url = 'https://api.telegram.org/bot' + bot_token + '/' + api_method + search;

	// Get the response from API.
	const response = await fetch(api_url, _request);

	const result = await response.text();

	const res = new Response(result, _request);

	res.headers.set('Content-Type', 'application/json');

	return res;
}

/**
 * Handles the incoming request.
 * @param {Request} request the incoming request.
 */
async function handleRequest(request) {
	const r = new Router();
	r.get(URL_PATH_REGEX, (req) => handler(req));
	r.post(URL_PATH_REGEX, (req) => handler(req));

	const resp = await r.route(request);
	return resp;
}

/**
 * Hook into the fetch event.
 */
addEventListener('fetch', (event) => {
	event.respondWith(handleRequest(event.request));
});
```

### 示例

![Screenshot_20250727-170615_.png](https://teleimg.loophy.top/file/Screenshot_20250727-170615_.png)

![image.png](https://teleimg.loophy.top/file/1753607282796_image.png)
