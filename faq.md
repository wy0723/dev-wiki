Q1:开发者是否可以直接把 token 是否有效做为用户是否登录的凭证?  

A:不可以,百度开放平台派发的 access\_token 会在某些情况下失效。开发者在获取了百度用户信 息后,应该依据百度用户信息自行维护用户的登录态\(session\),开发者应该保证 session 的安全 性并且不要设置过长的过期时间。    


Q2:获取 token 时返回 redirect\_uri\_mismatch  

A:该值必须与获取 Authorization  Code 时传递的“redirect\_uri”保持一致。

