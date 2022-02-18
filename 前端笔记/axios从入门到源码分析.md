# axios从入门到源码分析

## 1. axios的理解和使用

### 1.1 axios是什么

1. 前端最流行的ajax请求库

#### axios请求配置

> url: 请求路径
>
> method: 请求方式
>
> baseURL
>
> transformRequest: 对请求参数做一些预处理
>
> transformResponse: 对响应的结果做一些预处理
>
> headers: 请求头信息
>
> params: 设定url参数
>
> paramsSerializer: 对params格式做一些处理
>
> data: 
>
> * 对象型：自动转换成json格式字符串传递
> * 字符串型：直接传递
>
> timeout: 超时时间（单位：毫秒），默认值为0（没有超时时间）
>
> withCredentials: 设置跨域时是否携带cookies
>
> adapter: 对请求适配器做设置（发送ajax请求/在node.js中发送http请求）
>
> auth: （较少使用）请求的基础验证
>
> responseType: 设置响应体结果格式，默认为json
>
> responseEncoding: 响应体编码
>
> xsrfCookieName: 跨站请求标识，设置cookies的名字
>
> xsrfHeaderName: 跨站请求标识，设置头信息
>
> onUploadProgress: 上传时的回调
>
> onDownloadProgress: 下载时的回调
>
> maxContentLength: Http响应体的最大尺寸
>
> maxBodyLength: 请求体的最大尺寸
>
> validateStatus: 对响应结果的成功做一个设置，判断什么情况下为成功
>
> maxRedirects: 最大跳转次数（通常用于Node.js）
>
> socketPath: socket文件路径
>
> httpAgent: （较少使用）设置客户端信息
>
> httpsAgent: （较少使用）设置客户端信息
>
> proxy: 设置代理
>
> cancelToken: 对ajax请求做取消的设置
>
> decompress: 解压响应结果

#### axios返回结果

> config: 配置对象
>
> data: 响应体（axios自动将服务器返回结果进行json解析）
>
> headers: 响应头信息
>
> request: 原生Ajax请求对象（XMLHttpRequest实例对象）
>
> status: 响应状态码
>
> statusText: 响应状态字符串

#### axios拦截器

* 请求拦截器：对请求数据做处理，成功则放行
* 响应拦截器：对响应结果做处理，成功则放行

# 2. axios的源码分析

