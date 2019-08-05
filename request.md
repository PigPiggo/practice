/**
 * 基于Promise封装原生ajax的get方式和post方式
 */

;(function () {
    // 处理参数的函数
    function parseParam(params) {
        let str = '';
        for (let key in params) {
            str += `&${key}=${params[key]}`
        }
        return str.substr(1);
    }

    // 暴露一个对象
    window.request = {
        get (url) {
            return new Promise((resolve, reject) => {
                const xhr = new XMLHttpRequest();
                xhr.open('get', url);
                xhr.send();
                xhr.onreadystatechange = function () {
                    if (this.readyState === 4) {
                        if (this.status === 200) {
                            resolve( JSON.parse(this.responseText) )
                        } else {
                            reject( {code: this.status, reason: this.response} )
                        }
                    }
                }
            })
        },
        post(url, params) {
            return new Promise((resolve, reject) => {
                const xhr = new XMLHttpRequest();
                xhr.open('post', url);
                xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
                xhr.send( parseParam(params) );
                xhr.onreadystatechange = function () {
                    if (this.readyState === 4) {
                        if (this.status === 200) {
                            resolve( JSON.parse(this.responseText) )
                        } else {
                            reject( {code: this.status, reason: this.response} )
                        }
                    }
                }
            })
        }
    }

})()
