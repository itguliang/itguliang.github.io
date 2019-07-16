---
title: Node 微信公众号开发
date: 2018-11-01
categories:
  - IT
abbrlink: a6b8861e
---

### 1、服务器配置接入URL验证

```javascript
const TOKEN='你在微信公众号设置的token值'; 

function checkSignature(params,token){
    var key=[token,params.timestamp,params.nonce].sort().join(''); 
     //将token （自己设置的） 、timestamp（时间戳）、nonce（随机数）三个参数进行字典排序
    var sha1=require('crypto').createHash('sha1');
     //将上面三个字符串拼接成一个字符串再进行sha1加密
    sha1.update(key);
    return sha1.digest('hex') ==params.signature;
     //将加密后的字符串与signature进行对比，若成功，返回echostr
}

// url填写：地址接口/weixin 或者 域名/weixin
router.get('/weixin',(req,res) => {
    console.log(req.query)
    var params=req.query;
    if(!checkSignature(params,TOKEN)){
        //如果签名不对，结束请求并返回
        res.end('signature fail');
    }
    if (req.method == "GET") {
        //如果请求是GET，返回echostr用于通过服务器有效校验
        res.end(params.echostr);
    }else{
        //否则是微信给开发者服务器的POST请求
        var postdata='';
        req.addListener("data",function(postchunk){
            postdata+=postchunk;
        });
        //获取到了POST数据
        req.addListener("end",function(){
            console.log(postdata);
            res.end('success ');
        });
    }
});

// 可以将上述代码整合到某个项目后端代码中，或者单独项目都OK，启动服务访问

```
### 2、自定义消息回复

```javascript
const parseString = require('xml2js').parseString;

const messageJson=[
  "IT交流群：123493055",
  "要我照片？肿么可能，哈哈，上当了吧~"
];
const returnCon="感谢您的关注：\n加群回复【0】\n看我照片回复【1】";

function returnText(toUser, fromUser, content) {
    let xmlContent = `<xml><ToUserName><![CDATA[${toUser}]]></ToUserName>
    <FromUserName><![CDATA[${fromUser}]]></FromUserName>
    <CreateTime>${new Date().getTime()}</CreateTime>
    <MsgType><![CDATA[text]]></MsgType>
    <Content><![CDATA[${content}]]></Content></xml>`;
    return xmlContent;
}
function handleAutoReply(res, toUser, fromUser, keyword) {
    let messageMap = JSON.parse(JSON.stringify(messageJson));
    let content = messageJson[keyword];
    if (!content) {
        content=returnCon;
    }
    let xml = returnText(toUser, fromUser, content);
    res.send(xml); 
}
router.post('/weixin', (req, res) => {
    try {
        var postdata='';
        // 监听data事件，用于接收数据，用req.body是拿不到数据的
        req.on('data', (postchunk) => {
            postdata+=postchunk;
        });
        // 监听end事件，用于处理接收完成的数据
        req.on('end', () => {
            parseString(postdata, (err, result) => {
                // 处理错误
                if (err) {
                    console.log('解析微信服务器发来的消息出错了：');
                    console.log(err);
                    res.send('success');
                    return false;
                }
                if (!result || !result.xml) {
                    // 未接收到有效消息，告诉微信服务器不要再尝试连接
                    res.send('success');
                    return console.log('未接收到任何消息也未发生任何事件');
                }

                result = result.xml;
                // 接收方微信（注意接收方和发送方的转换）
                let toUser = result.FromUserName;
                // 发送方微信
                let fromUser = result.ToUserName;
                let userMessage = result.Content;
                console.log('-----------------------开始处理消息-----------------------');
                if (result.Event == 'subscribe') {
                    console.log('--------------------有用户关注了---------------------------');
                    handleAutoReply(res, toUser, fromUser, returnCon);
                } else {
                    // 其他消息
                    if (result.MsgType != 'text') {
                        res.send('success');
                        console.log('------------------不是文本类型的消息暂不处理----------------------');
                        return false;
                    }
                    // 文本消息
                    // 这里可以处理一些特殊回复，比如发送编码查询等
                    // 处理关键词自动回复
                    console.log('-----------------------现在处理关键词回复------------------------');
                    handleAutoReply(res, toUser, fromUser, userMessage);
                }
            });
        });
    } catch(err) {
        console.log(err);
        res.send('success');
    }
});
```