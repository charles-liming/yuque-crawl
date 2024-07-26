# yuque-crawl
语雀文档抓取工具（爬虫） 可以保存任意用户整个语雀知识库为Markdown格式 (包含完整目录结构和索引) 

![](yuque-demo.png)


# 使用：
## 安装 python3

https://www.python.org/downloads/

## 执行安装运行模块
`pip install requests`

## 执行抓取：

`python3 main.py 语雀文档地址`

demo：
```
python3 main.py https://www.yuque.com/burpheart/phpaudit
```



# 爬取阳明的k8s 笔记  https://www.yuque.com/cnych/k8s4
## 第一步：获取cookies ,前提是可以访问阳明的K8S 文档，然后通过浏览器开发者工具的Network ,获取cookie ，
<img width="895" alt="image" src="https://github.com/user-attachments/assets/e237412d-271a-4465-8b75-c7ee932370c4">
## 然后，拿到一个curl 方法

```
curl 'https://www.yuque.com/cnych/k8s4' \
  -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7' \
  -H 'Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,und;q=0.7,fr;q=0.6,zh-TW;q=0.5' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Cookie: aliyungf_tc=0e6ad2b64a32c95b983372005bd7821530c51b2ff8a411f0c4e9a47486df177d; yuque_ctoken=J7QemvmMLmjjzddPuEcfSNZE; receive-cookie-deprecation=1; lang=zh-cn; _yuque_session=Wp4jcnzLxQz_Ipaz8d5WOJpml5BFzBraLvRRdjSts-cILODZ4WmwiQdfPd0extmESSLEBDmDLHU-cR7sT1LLSw==; _uab_collina=172190028131965544945931; tfstk=f4jo_IXWKIG_7mMLZix7Rb2y8oUvFbtBP6np9HdUuIRjwbnRLkoFpObJ87tRxsffG_BRpMbjxO6C28tLF36WAHPT6lIhFTtQBVIxCg0q0pBq44JzPs4dUHPT6oHFAnB2YweiEHxc3I9HLp8FTjJ2GdvyYQ8EgqJwgB-eY68q3pvZa48EzEoV7Ok2w65507DFsN3dLDI2EUAnYKoHxio9lCWJi2lPmLAe_TRmYDA7Ph0cL6Pnvh71iMYwig-juVolsD94piuIRUJXnCg9Fi57wpv2E-2m7NTyhLrTn-0IeUJXnCe0nVlyzK9y6; current_theme=default; acw_tc=ac11000117219729720336695e8948c083937a57dd98a31322ba12963a3b9f' \
  -H 'Pragma: no-cache' \
  -H 'Sec-Fetch-Dest: document' \
  -H 'Sec-Fetch-Mode: navigate' \
  -H 'Sec-Fetch-Site: none' \
  -H 'Sec-Fetch-User: ?1' \
  -H 'Service-Worker-Navigation-Preload: true' \
  -H 'Upgrade-Insecure-Requests: 1' \
  -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36' \
  -H 'sec-ch-ua: "Not/A)Brand";v="8", "Chromium";v="126", "Google Chrome";v="126"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"'
```

## 复制cookies 部分到cookie 解析网站
https://www.kgtools.cn/compression/cookie
<img width="1372" alt="image" src="https://github.com/user-attachments/assets/c9674b6f-82f7-4d30-abdc-32d32dfe120a">

## 然后修改脚本yangming-k8s4-crawl.py 中的cookie 部分。
<img width="1178" alt="image" src="https://github.com/user-attachments/assets/59c0422f-13c4-4d29-8b0d-305845a62513">


```
python3 yangming-k8s4-crawl.py https://www.yuque.com/cnych/k8s4
```
