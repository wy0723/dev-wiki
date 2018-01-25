**Q1：开发者是否可以直接把token是否有效做为用户是否登录的凭证？**

A：不可以，百度开放平台派发的access\_token会在某些情况下失效。开发者在获取了百度用户信息后，应该依据百度用户信息自行维护用户的登录态（session），开发者应该保证session的安全性并且不要设置过长的过期时间。

**Q2：获取token时返回redirect\_uri\_mismatch**

A：该值必须与获取Authorization  Code时传递的“redirect\_uri”保持一致。

