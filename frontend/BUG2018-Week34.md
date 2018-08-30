## 本周总结 AF
1. bug总结
    - NodeJs的异步导致数据传输时为空
    	- 用Promise和Sync解决了异步数据传输的问题
    	```NodeJS
    	function doGetId(keywords) {
          return rp({
            uri: 'https://www.douban.com/search?source=suggest',
            qs: { q: keywords },
            headers: {
              'User-Agent': 'Request-Promise'
            }
          }).then(body => {
            const $ = cheerio.load(body)
            const items = []
            $('#content .result-list').first().children().each(function(idx, element) {
              var $element = $(element)
              // console.log($element);
              var str = $element.find('.result a').attr('onclick')
              if (str != null) {
                var count = str.indexOf('sid')
                var countTwo = str.indexOf('qcat')
                var strcopy = str.slice(count + 5, countTwo - 2)
              }
              items.push({
                href: $element.find('.result a').attr('href'),
                id: strcopy,
                types: $element.find('h3 span').text(),
                title: $element.find('.title a').text()
              })
            })
            console.log(items)
            if (items === null) {
              Message({
                message: '未找到相关信息',
                type: 'error',
                duration: 5 * 1000
              })
            }
            return items
          }).catch((err) => {
            // POST failed...
            console.log('spider request error:', err)
            return err.statusCode
          })
         }
        ```
        
    - 文件夹命名错误
        - 写代码的时候还要更加细心，避免犯这种低级错误
        - ==SQL语句一定要细心写！！==
    - 前台接受数据时的格式问题
        - 目前还要靠JSON.stringify()和JSON.parse()来试错式调试
   
    
