# **1. 数字操作**

## **（1）生成指定范围随机数**

```
export const randomNum = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
const a = randomNum(1,99) 
```

## **（1）数字千分位分隔**

```
 const format = (n) => {
    let num = n.toString();
    let len = num.length;
    if (len <= 3) {
        return num;
    } else {
        let temp = '';
        let remainder = len % 3;
        if (remainder > 0) { // 不是3的整数倍
            return num.slice(0, remainder) + ',' + num.slice(remainder, len).match(/\d{3}/g).join(',') + temp;
        } else { // 3的整数倍
            return num.slice(0, len).match(/\d{3}/g).join(',') + temp;
        }
    }
}
const b = format(97.442)
 console.log(b)  //442
```

# **2. 数组操作**

## **（1）数组乱序**

```
 const arrScrambling = (arr) => {
    for (let i = 0; i < arr.length; i++) {
        const randomIndex = Math.round(Math.random() * (arr.length - 1 - i)) + i;
        [arr[i], arr[randomIndex]] = [arr[randomIndex], arr[i]];
    }
    return arr;
}

const a = arrScrambling([1, 2, 3, 4, 5, 6, 7, 8])
 console.log(a)
通过使用 Math.random() 生成一个0到1之间的随机数，乘以 (arr.length - 1 - i)，得到一个在当前位置 i 到数组末尾之间的随机浮点数。

 使用 Math.round() 对随机浮点数进行四舍五入，得到最接近的整数，作为 randomIndex 的值。

 通过解构赋值的方式，交换位置 i 和 randomIndex 处的数组元素，实现乱序。
```

## **（2）数组扁平化**

```
 const flatten = (arr) => {
    let result = [];

    for(let i = 0; i < arr.length; i++) {
        if(Array.isArray(arr[i])) {
            result = result.concat(flatten(arr[i]));
        } else {
            result.push(arr[i]);
        }
    }
    return result;
}
const a = [11,2,[3],[4,5],[6,7],[99,[888]]]
 console.log(flatten(a)) // [11, 2,  3,   4, 5,6, 7, 99, 888]
 console.log(a.flat(Infinity)) // [11, 2,  3,   4, 5,6, 7, 99, 888]
```

## **（3）数组中获取随机数**

```
const sample = arr => arr[Math.floor(Math.random() * arr.length)];
const a = [1, 2, 3, 4, 5]
console.log(sample(a))
```

# **3. 字符串操作**

## **（1）生成随机位数字符串**



```
 const randomString = (len) => {
    let chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz123456789';
    let strLen = chars.length;
    let randomStr = '';
    for (let i = 0; i < len; i++) {
        randomStr += chars.charAt(Math.floor(Math.random() * strLen));
    }
    return randomStr;
};
 console.log(randomString(5))
```

## **（2）字符串首字母大写**

```
 const fistLetterUpper = (str) => {
    return str.charAt(0).toUpperCase() + str.slice(1);
};
 console.log(fistLetterUpper('adewfsfg')) //Adewfsfg
```

## **（3）手机号中间四位变成\***



```
 const telFormat = (tel) => {
    tel = String(tel);
    return tel.substr(0,3) + "****" + tel.substr(7);
};
 console.log(telFormat('15396587235'))  //153****7235
```

## **（4）驼峰命名转换成短横线命名**



```
 const getKebabCase = (str) => {
    return str.replace(/[A-Z]/g, (item) => '-' + item.toLowerCase())
}

 console.log(getKebabCase('addListShow'))   //add-list-show
```

## **（5）短横线命名转换成驼峰命名**

```
 const getCamelCase = (str) => {
    return str.replace( /-([a-z])/g, (i, item) => item.toUpperCase())
}

 console.log(getCamelCase('add-list-show'))  //addListShow
```

# **4. 格式转化**

## **（1）数字转化为大写金额**

```
 const digitUppercase = (n) => {
    const fraction = ['角', '分'];
    const digit = [
        '零', '壹', '贰', '叁', '肆',
        '伍', '陆', '柒', '捌', '玖'
    ];
    const unit = [
        ['元', '万', '亿'],
        ['', '拾', '佰', '仟']
    ];
    n = Math.abs(n);
    let s = '';
    for (let i = 0; i < fraction.length; i++) {
        s += (digit[Math.floor(n * 10 * Math.pow(10, i)) % 10] + fraction[i]).replace(/零./, '');
    }
    s = s || '整';
    n = Math.floor(n);
    for (let i = 0; i < unit[0].length && n > 0; i++) {
        let p = '';
        for (let j = 0; j < unit[1].length && n > 0; j++) {
            p = digit[n % 10] + unit[1][j] + p;
            n = Math.floor(n / 10);
        }
        s = p.replace(/(零.)*零$/, '').replace(/^$/, '零') + unit[0][i] + s;
    }
    return s.replace(/(零.)*零元/, '元')
        .replace(/(零.)+/g, '零')
        .replace(/^整$/, '零元整');
};
 console.log(digitUppercase(12941799.52))  //壹仟贰佰玖拾肆万壹仟柒佰玖拾玖元伍角贰分
```

# **5. 格式校验**

## **（1）校验身份证号码**

```
export const checkCardNo = (value) => {
    let reg = /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/;
    return reg.test(value);
};
```

## **（2）校验是否包含中**文

```
export const haveCNChars => (value) => {
    return /[\u4e00-\u9fa5]/.test(value);
}
```

## **（3）校验是否为中国大陆的邮政编码**

```
export const isPostCode = (value) => {
    return /^[1-9][0-9]{5}$/.test(value.toString());
}
```

## **（4）校验是否为IPv6地址**

```
export const isIPv6 = (str) => {
    return Boolean(str.match(/:/g)?str.match(/:/g).length<=7:false && /::/.test(str)?/^([\da-f]{1,4}(:|::)){1,6}[\da-f]{1,4}$/i.test(str):/^([\da-f]{1,4}:){7}[\da-f]{1,4}$/i.test(str));
}
```

## **（5）校验是否为邮箱地址**

```
export const isEmail = (value) {
    return /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/.test(value);
}
```

## **（6）校验是否为中国大陆手机号**

```
export const isTel = (value) => {
    return /^1[3,4,5,6,7,8,9][0-9]{9}$/.test(value.toString());
}
```

## **（7）校验是否包含emoji表情**

```
export const isEmojiCharacter = (value) => {
  	value = String(value);
    for (let i = 0; i < value.length; i++) {
        const hs = value.charCodeAt(i);
        if (0xd800 <= hs && hs <= 0xdbff) {
            if (value.length > 1) {
                const ls = value.charCodeAt(i + 1);
                const uc = ((hs - 0xd800) * 0x400) + (ls - 0xdc00) + 0x10000;
                if (0x1d000 <= uc && uc <= 0x1f77f) {
                    return true;
                }
            }
        } else if (value.length > 1) {
            const ls = value.charCodeAt(i + 1);
            if (ls == 0x20e3) {
                return true;
            }
        } else {
            if (0x2100 <= hs && hs <= 0x27ff) {
                return true;
            } else if (0x2B05 <= hs && hs <= 0x2b07) {
                return true;
            } else if (0x2934 <= hs && hs <= 0x2935) {
                return true;
            } else if (0x3297 <= hs && hs <= 0x3299) {
                return true;
            } else if (hs == 0xa9 || hs == 0xae || hs == 0x303d || hs == 0x3030
                    || hs == 0x2b55 || hs == 0x2b1c || hs == 0x2b1b
                    || hs == 0x2b50) {
                return true;
            }
        }
    }
  	return false;
}
```

# **6. 操作URL**

## **（1）获取URL参数列表**

```
export const GetRequest = () => {
    let url = location.search;
    const paramsStr = /.+\?(.+)$/.exec(url)[1]; // 将 ? 后面的字符串取出来
    const paramsArr = paramsStr.split('&'); // 将字符串以 & 分割后存到数组中
    let paramsObj = {};
    // 将 params 存到对象中
    paramsArr.forEach(param => {
      if (/=/.test(param)) { // 处理有 value 的参数
        let [key, val] = param.split('='); // 分割 key 和 value
        val = decodeURIComponent(val); // 解码
        val = /^\d+$/.test(val) ? parseFloat(val) : val; // 判断是否转为数字
        if (paramsObj.hasOwnProperty(key)) { // 如果对象有 key，则添加一个值
          paramsObj[key] = [].concat(paramsObj[key], val);
        } else { // 如果对象没有这个 key，创建 key 并设置值
          paramsObj[key] = val;
        }
      } else { // 处理没有 value 的参数
        paramsObj[param] = true;
      }
    })
    return paramsObj;
};
```

## **（2）检测URL是否有效**

```
export const getUrlState = (URL) => {
  let xmlhttp = new ActiveXObject("microsoft.xmlhttp");
  xmlhttp.Open("GET", URL, false);
  try {
    xmlhttp.Send();
  } catch (e) {
  } finally {
    let result = xmlhttp.responseText;
    if (result) {
      if (xmlhttp.Status == 200) {
        return true;
      } else {
        return false;
      }
    } else {
      return false;
    }
  }
}
```

## **（3）键值对拼接成URL参数**

```
export const params2Url = (obj) => {
     let params = []
     for (let key in obj) {
       params.push(`${key}=${obj[key]}`);
     }
     return encodeURIComponent(params.join('&'))
}
```

## **（4）修改URL中的参数**

```
export const replaceParamVal => (paramName, replaceWith) {
   const oUrl = location.href.toString();
   const re = eval('/('+ paramName+'=)([^&]*)/gi');
   location.href = oUrl.replace(re,paramName+'='+replaceWith);
   return location.href;
}
```

## **（5）删除URL中指定参数**

```
export const funcUrlDel = (name) => {
  const baseUrl = location.origin + location.pathname + "?";
  const query = location.search.substr(1);
  if (query.indexOf(name) > -1) {
    const obj = {};
    const arr = query.split("&");
    for (let i = 0; i < arr.length; i++) {
      arr[i] = arr[i].split("=");
      obj[arr[i][0]] = arr[i][1];
    }
    delete obj[name];
    return baseUrl + JSON.stringify(obj).replace(/[\"\{\}]/g,"").replace(/\:/g,"=").replace(/\,/g,"&");
  }
}
```

# **7. 设备判断**

## **（1）判断是移动还是PC设备**

```
export const isMobile = () => {
  if ((navigator.userAgent.match(/(iPhone|iPod|Android|ios|iOS|iPad|Backerry|WebOS|Symbian|Windows Phone|Phone)/i))) {
		return 'mobile';
  }
  return 'desktop';
}
```

## **（2）判断是否是苹果还是安卓移动设备**

```
export const isAppleMobileDevice = () => {
  let reg = /iphone|ipod|ipad|Macintosh/i;
  return reg.test(navigator.userAgent.toLowerCase());
}
```

## **（3）判断是否是安卓移动设备**

```
export const isAndroidMobileDevice = () => {
  return /android/i.test(navigator.userAgent.toLowerCase());
}
```

## **（4）判断是Windows还是Mac系统**

```
export const osType = () => {
    const agent = navigator.userAgent.toLowerCase();
    const isMac = /macintosh|mac os x/i.test(navigator.userAgent);
  	const isWindows = agent.indexOf("win64") >= 0 || agent.indexOf("wow64") >= 0 || agent.indexOf("win32") >= 0 || agent.indexOf("wow32") >= 0;
    if (isWindows) {
        return "windows";
    }
    if(isMac){
        return "mac";
    }
}
```

## **（5）判断是否是微信/QQ内置浏览器**

```
export const broswer = () => {
    const ua = navigator.userAgent.toLowerCase();
    if (ua.match(/MicroMessenger/i) == "micromessenger") {
        return "weixin";
    } else if (ua.match(/QQ/i) == "qq") {
        return "QQ";
    }
    return false;
}
```

## **（6）浏览器型号和版本**

```
export const getExplorerInfo = () => {
    let t = navigator.userAgent.toLowerCase();
    return 0 <= t.indexOf("msie") ? { //ie < 11
        type: "IE",
        version: Number(t.match(/msie ([\d]+)/)[1])
    } : !!t.match(/trident\/.+?rv:(([\d.]+))/) ? { // ie 11
        type: "IE",
        version: 11
    } : 0 <= t.indexOf("edge") ? {
        type: "Edge",
        version: Number(t.match(/edge\/([\d]+)/)[1])
    } : 0 <= t.indexOf("firefox") ? {
        type: "Firefox",
        version: Number(t.match(/firefox\/([\d]+)/)[1])
    } : 0 <= t.indexOf("chrome") ? {
        type: "Chrome",
        version: Number(t.match(/chrome\/([\d]+)/)[1])
    } : 0 <= t.indexOf("opera") ? {
        type: "Opera",
        version: Number(t.match(/opera.([\d]+)/)[1])
    } : 0 <= t.indexOf("Safari") ? {
        type: "Safari",
        version: Number(t.match(/version\/([\d]+)/)[1])
    } : {
        type: t,
        version: -1
    }
}
```



# **8 浏览器操作**

## **（1）滚动到页面顶部**

```
export const scrollToTop = () => {
  const height = document.documentElement.scrollTop || document.body.scrollTop;
  if (height > 0) {
    window.requestAnimationFrame(scrollToTop);
    window.scrollTo(0, height - height / 8);
  }
}
```

## **（2）滚动到页面底部**

```
export const scrollToBottom = () => {
  window.scrollTo(0, document.documentElement.clientHeight);  
}
```

## **（3）滚动到指定元素区域**

```
export const smoothScroll = (element) => {
    document.querySelector(element).scrollIntoView({
        behavior: 'smooth'
    });
};
```

## **（4）获取可视窗口高度**

```
export const getClientHeight = () => {
    let clientHeight = 0;
    if (document.body.clientHeight && document.documentElement.clientHeight) {
        clientHeight = (document.body.clientHeight < document.documentElement.clientHeight) ? document.body.clientHeight : document.documentElement.clientHeight;
    }
    else {
        clientHeight = (document.body.clientHeight > document.documentElement.clientHeight) ? document.body.clientHeight : document.documentElement.clientHeight;
    }
    return clientHeight;
}
```

## **（5）获取可视窗口宽度**

```
export const getPageViewWidth = () => {
    return (document.compatMode == "BackCompat" ? document.body : document.documentElement).clientWidth;
}
```

## **（6）打开浏览器全屏**

```
export const toFullScreen = () => {
    let element = document.body;
    if (element.requestFullscreen) {
      element.requestFullscreen()
    } else if (element.mozRequestFullScreen) {
      element.mozRequestFullScreen()
    } else if (element.msRequestFullscreen) {
      element.msRequestFullscreen()
    } else if (element.webkitRequestFullscreen) {
      element.webkitRequestFullScreen()
    }
}
```

## **（7）退出浏览器全屏**

```
export const exitFullscreen = () => {
    if (document.exitFullscreen) {
      document.exitFullscreen()
    } else if (document.msExitFullscreen) {
      document.msExitFullscreen()
    } else if (document.mozCancelFullScreen) {
      document.mozCancelFullScreen()
    } else if (document.webkitExitFullscreen) {
      document.webkitExitFullscreen()
    }
}
```