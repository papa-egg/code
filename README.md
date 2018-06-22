![Async Logo](https://yunxi-material-library.oss-cn-hangzhou.aliyuncs.com/recordImg/786/EsSfKfAaMz.png)

JavaScript算法题([ES6](http://es6.ruanyifeng.com/))，不定期更新。。。

个人收藏和积累，思维逻辑和编码风格（仅供参考）--- 如有摘抄，请注明出处。

我很欢迎别人能在自己拙劣的代码里获得一些收获、get一些技巧和代码思路等等。

*希望你能够喜欢上JS这一门非常有特色的编程语言([ES6](http://es6.ruanyifeng.com/)也给js注入了一种简洁和优雅)，并努力去学习掌握它*


```javascript
// demo...
    const sr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    const rel = [];

    function getCNum(ss) {
        let _s = 0;

        for (let item of ss) {
            _s += parseInt(item);
        }

        return _s;
    }

    function cycleCumulative(ci = 0, ss = '') {
        for (let i = ci; i < sr.length; i++) {
            const _cs = getCNum(ss);
            const _cd = ss + sr[i];

            if (_cd.length == k) {
                if (_cs + sr[i] == n) {
                    const ra = [];

                    [..._cd].forEach((item) => {
                        ra.push(parseInt(item));
                    });

                    rel.push(ra);
                }
            } else {
                cycleCumulative(sr[i], _cd);
            }
        }
    }
```
