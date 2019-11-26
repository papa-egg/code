# [JavaScript code](https://github.com/papa-egg/code/blob/master/code.js)

JavaScript算法题（偏困难）个人逻辑思路和对应编码解答([ES6](http://es6.ruanyifeng.com/))，问题主要来自于[leetcode](https://leetcode-cn.com),不定期更新。。。

* 个人收藏和积累，思维逻辑和编码风格（仅供参考）--- 如有摘抄，请注明出处。

* 我很欢迎别人能在自己拙劣的代码里获得一些收获、get一些技巧和代码思路等等。

*希望你能够喜欢上JS这一门非常有特色的编程语言(有了[ES6](http://es6.ruanyifeng.com/)之后也变得更加简洁、优雅)，并努力去学习掌握它*


```javascript

/**
 * 等差数列含有一序列的数项，所有后一项减去前一项的差值都想吐
 * 例如，对于序列3,7,11,15；每一项减去前一项得到4。给定你一系列整数，我们要找到通过从给定系列整数
 * 中选择部分数字出来组成等差数列(可以交换数字顺序,可以选取所有数字)，并从中找到最长的等差数列
 * 例如[3,8,4,5,6,2,2],从这些数字中选取一些数字所组成的最长等差数列是2,3,4,5,6
 *
 * @param { Array } args
 * @return { Number }
 */
function getMaxEqualDiffLength(args) {
    const numArgs = Array.from(new Set(args.sort((a, b) => a - b)));
    const maxDiff = numArgs[numArgs.length - 1] - numArgs[0];
    const rel = [];
    let maxLength = 0;

    function getMaxDiffStr(curStr, curNum, diff) {
        let addNum = curNum + diff;

        if (numArgs.indexOf(addNum) > -1) {
            curStr += '|' + addNum;
            curNum = curNum + diff;

            getMaxDiffStr(curStr, curNum, diff);
        } else {
            let curStrLength = curStr.split('|').length;

            maxLength = curStrLength > maxLength ? curStrLength : maxLength;
            if (curStr.split('|').length > 1) rel.push(curStr);
        }
    }

    for (let i = 1; i <= maxDiff; i++) {
        for (let j = 0; j < numArgs.length; j++) {
            let curStr = '' + numArgs[j];
            let curNum = parseInt(numArgs[j]);

            getMaxDiffStr(curStr, curNum, i);
        }
    }

    rel.sort((a, b) => b.split('|').length - a.split('|'));

    return maxLength;
}

/**
 * 给予包含若干整数的数组nums,查找数组中所有唯一的三元组，它们的总和为零
 * 例：
 * 给定数组nums = [-1,0,1,2,-1,-4]，
 * 解决方案集是：
 * [
 * [-1,0,1]，
 * [-1,-1,2]
 * ]
 *
 * @param { Array } nums
 * @return { Array }
 */
function threeSum(nums) {
    const rel = [];

    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {

            nums.forEach((item, index) => {
                if (index == i || index == j) return;

                if (nums[i] + nums[j] + item == 0) {

                    const unite = rel.every((relItem) => {
                        if ((relItem.indexOf(nums[i]) > -1 &&
                            relItem.indexOf(nums[j]) > -1 &&
                            relItem.indexOf(item) > -1)
                        ) {
                            const newArr = Object.assign([], relItem);

                            if (nums[i] == 0 && nums[j] == 0 && item == 0) {
                                if (relItem[0] || relItem[1] || relItem[2]) {
                                    return true;
                                }
                            }

                            return false;
                        }

                        return true;
                    });

                    if (unite) {
                        rel.push([nums[i], nums[j], item]);
                    }
                }
            })
        }
    }

    return rel;
}

/**
 * 果一个正整数不能被大于1的完全平方数所整除，那么我们就将该数称为无平方数因子的数
 * 例如，靠前的一些无平方数因数的数是{1,2,3,5,6,7,10,11,13,14,15,17,19…}
 * 给出一个整数n,返回第n个无平方数因子的数
 *
 * @param { Number } n The integer 1-1,000,000
 * @return { Number }
 */
function getSqrtNum(n) {
    const rel = [1];
    let cIndex = 2;

    function isNotCompleteSqrt(num) {
        const sqrtNum = Math.sqrt(num);

        return !(sqrtNum == parseInt(sqrtNum));
    }

    function increaseCal(cIndex) {
        if (rel.length == n) return;

        if (isNotCompleteSqrt(cIndex)) rel.push(cIndex);

        increaseCal(++cIndex);
    }

    increaseCal(cIndex);

    return rel.pop();
}

/**
 * 找出这样的数：
 *  1. 6位正整数
 *  2. 每个数位上的数字不同
 *  3. 其平方数的每个数位不含原数字的任何组成数位
 *   如:　203879 * 203879 = 41566646641
 *
 * @return { Array }
 */
function getOutQuareNum() {
    const rel = [];

    function isOutQuareNum(n) {
        const initSet = [...new Set(String(n).split(''))];
        const sqrSet = [...new Set(String(n * n).split(''))];
        const finSet = [...new Set([...initSet, ...sqrSet])];

        if (initSet.length == String(n).length &&
            finSet.length == initSet.length + sqrSet.length) {
            return true;
        }

        return false;
    }

    for (let i = 100000; i <= 999999; i++) {
        if (isOutQuareNum(i)) {
            rel.push(i);
        }
    }

    return rel;
}

/**
 * 有一对兔子，从出生后第3个月起每个月都生一对兔子，小兔
 * 子长到第三个月后每个月又生一对兔子，假如兔子都不死，问每个月的兔子总数
 * 为多少？
 * 兔子的规律为数列1,1,2,3,5,8,13,21....
 *
 * @param { Number } n
 * @return { Number }
 */
function calRabbitNum(n = 0) {
    if (typeof +n != 'number' || n <= 0) {
        throw new Error('n is not the integer');
    }
    let fertList = [{ m: 3, s: false }];
    let finNum = 1;
    let addNum = 0;

    for (let i = 1; i <= n; i++) {
        for (let fm of fertList) {
            if (fm.m <= i && (!fm.s)) {
                fm.s = true;
                addNum++;
            }
        }

        if (addNum > 0) {
            for (let j = 0; j < addNum; j++) {
                fertList.push({
                    m: i + 2,
                    s: false,
                });
            }
        }

        finNum += addNum;
    }

    return finNum;
}

/**
 * 给定一个字符串，找到最长的子字符串的长度而不重复字符
 * 例子: 给定"abcabcbb"的答案是"abc"，长度是3
 *
 * @param { String } s
 * @return { Number }
 */
function lengthOfLongestSubstring(s) {
    const sr = s.split('');
    let rel = '';
    let l = 0;

    function calLongStr(sr) {
        let _s = '';

        for (let [index, item] of sr.entries()) {
            if (_s.indexOf(item) == -1) {
                _s += item;
            } else {
                if (_s.length > l) {
                    rel = _s;
                    l = _s.length;
                }

                if (sr.slice(index).length > l) {
                    calLongStr(sr.slice(index));
                }

                break;
            }

            rel = _s;
            l = _s.length;
        }
    }

    calLongStr(sr);

    return l;
}

/**
 * 找到的所有可能的组合ķ数字加起来为数字n鉴于从1到9的数字
 * 仅可以使用与每个组合应该是唯一的一组数字
 * 例：输入： k = 3， n = 9
 * 输出：[[1,2,6]，[1,3,5]，[2,3,4]]
 *
 * @param { Number } k
 * @param { Number } n
 * @return { Array }
 */
function combinationSum3(k, n) {
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

    cycleCumulative();

    return rel;
}

/**
 * 一个字符串，其数字可以形成加法序列。
 * 有效的附加序列应至少包含三个数字。除前两个数字外，序列中的每个后续数字必须是前两个数字的和。
 * 例如："112358"是一个加号，因为数字可以形成一个加法序列：1, 1, 2, 3, 5, 8。
 * [1 + 1 = 2], [1 + 2 = 3], [2 + 3 = 5], [3 + 5 = 8]
 * 注意：添加序列中的数字不能有前导零，因此序列1, 2, 03或1, 02, 3无效。
 * 给定一个只包含数字的字符串'0'-'9'，编写一个函数来确定它是否是一个附加数字。
 *
 * @param { String } num
 * @return { Boolean }
 */
function isAdditiveNumber(num) {
    let nr = num.split('');
    let rr = [];
    let rq = [];
    let rel = [];

    for (let i = 2; i <= nr.length - 1; i++) {
        putLocal(1, '', i);
    }

    function putLocal(s, sr = '', n = 1) {
        for (let i = s; i <= nr.length - 1; i++) {
            let sAr = !sr ? [] : sr.split(',');

            if (sAr.indexOf(String(i)) == -1) {
                sAr.push(String(i));

                if (sAr.length == n && sAr.length > 1) {
                    rr.push(sAr.join(','));
                } else {
                    putLocal(i + 1, sAr.join(','), n);
                }
            }
        }
    }

    function toInt(num) {
        return +num;
    }

    for (let i = 0; i < rr.length; i++) {
        const _cr = [];
        const _nr = Object.assign([], nr);
        const _ra = rr[i].split(',');

        for (let j = 0; j < _ra.length; j++) {
            if (j == 0) {
                _cr.push((_nr.splice(0, (_ra[j]))).join(''));
            } else {
                _cr.push((_nr.splice(0, (_ra[j]) - _ra[j - 1])).join(''));
            }
        }

        _cr.push(_nr.join(''));
        rq.push(_cr);
    }

    for (let i = 0; i < rq.length; i++) {
        const cArr = rq[i];

        if (cArr.length >= 3) {
            let isPass = true;

            for (let j = 0; j < cArr.length; j++) {
                if (j + 3 <= cArr.length) {

                    // filtering pre - 0
                    if (!(String(parseInt(cArr[j])) == cArr[j]
                        && String(parseInt(cArr[j + 1])) == cArr[j + 1]
                        && String(parseInt(cArr[j + 2])) == cArr[j + 2])
                    ){
                        isPass = false;
                    }

                    if (toInt(cArr[j]) + toInt(cArr[j + 1]) != toInt(cArr[j + 2])) {
                        isPass = false;
                    }
                }
            }

            if (isPass) {
                rel.push(cArr);
            }
        }
    }

    return rel.length > 0;
}

/**
 * 给定一个填充非负数的m × n网格，找到一条从左上角到右下角的路径，该路径使沿路径上的所有数字的总和最小。
 * 注意：您只能在任何时间点向下或向右移动。
 * 输入：
 *  [1,3,1]
 *  [1,5,1]
 *  [4,2,1]
 * 输出： 7  说明：因为路径1→3→1→1→1使总和最小化
 *
 * @param { Array } grid
 * @return { Number }
 */
function minPathSum(grid) {
    const rel = [];

    function snakeWalk(x, y, s) {
        if (grid[x + 1]) {
            snakeWalk(x + 1, y, s + grid[x + 1][y]);
        }

        if (grid[x][y + 1]) {
            snakeWalk(x, y + 1, s + grid[x][y + 1])
        }

        if ((!grid[x + 1]) && (!grid[x][y + 1])) {
            rel.push(s);
        }

    }

    snakeWalk(0, 0, grid[0][0]);
    rel.sort((a, b) => a - b);

    return rel[0];
}

/**
 * 给予A和B两个整数数组,返回出现在两个数组中子数组的最大长度
 * 例 如:
 * 输入:
 * A: [1,2,3,2,1]
 * B: [3,2,1,4,7]
 * 输出: 3
 * 解答:
 * 相同最大子数组为 [3, 2, 1]
 *
 * @param { Array } A
 * @param { Array } B
 * @return { Number }
 */
function findLength(A, B) {
    const rel = [];
    let mx = 0;

    function setSameArr(num) {
        for (let [index, item] of A.entries()) {
            const sr = A.slice(index, index + num);

            if (sr.length == num) {
                rel.push(sr);
            }
        }
    }

    for (let i = 1; i <= A.length; i++) {
        setSameArr(i);
    }

    for (let item of rel) {
        const sr = '|' + item.join('|') + '|';
        const sa = '|' + A.join('|') + '|';
        const sb = '|' + B.join('|') + '|';

        if (sa.indexOf(sr) > -1 && sb.indexOf(sr) > -1 && item.length > mx) {
            mx = item.length;
        }
    }

    return mx;
}

/**
 * 诱捕雨水
 * 给定n个非负整数来表示每个柱的宽度为1的高程图，计算下雨后它能够捕集多少水。
 * 例如: 给定 [0,1,0,2,1,0,1,3,2,1,2,1]，返回6
 *
 * @param { Array } nums
 * @return { Number }
 */
function trap(nums) {
    let nArr = [];
    let caps = [];
    let s = 0;

    function calHandler(i = 0) {
        if (typeof nums[i] != 'number') return;

        if (caps.length <= 0) {
            const _smax = Math.max(...nums.slice(i + 1));

            if (nums[i] >= _smax) {
                const _next = nums.indexOf(_smax, i + 1);

                nArr.push([i, _next]);
                caps = [];
                calHandler(_next);
            } else {
                caps.push(i);
                calHandler(++i);
            }
        } else {
            if (nums[i] < nums[caps[0]]) {
                calHandler(++i);
            } else {
                nArr.push([caps[0], i]);
                caps = [];
                calHandler(i);
            }
        }
    }

    calHandler();

    for (let item of nArr) {
        const _crr = item;
        const _cmax = Math.min(nums[_crr[0]], nums[_crr[[1]]]);

        for (let i = _crr[0] + 1; i < _crr[1]; i++) {
            s += _cmax - nums[i];
        }
    }

    return s;
}

/**
 * 给定一个字符串s，您可以通过在其前添加字符将其转换为回文。
 * 通过执行此转换，查找并返回可找到的最短回文。
 * 例如：
 *   输入："aacecaaa" 输出： "aaacecaaa"
 *   输入："abbacd" 输出： "dcabbacd"
 *
 * @param { String } s
 * @return { String }
 */
function shortestPalindrome(s) {
    if (s.length <= 1) return s;
    const sArr = s.split('').join(' ').split('');
    const rel = [];

    function generateShortes(cr) {
        const mArr = (sArr.length - 1 - cr > cr) ? sArr.slice(cr + 1) : sArr.slice(0, cr).reverse();
        const xArr = (sArr.length - 1 - cr > cr) ? sArr.slice(0, cr).reverse() : sArr.slice(cr + 1);
        let isShort = true;

        for (let [index, item] of mArr.entries()) {
            const _cx = item;
            const _cy = xArr[index] || item;

            if (_cx == _cy) {
                xArr[index] = item;
            } else {
                isShort = false;

                break;
            }
        }

        if (isShort) {
            rel.push([...xArr.reverse(), sArr[cr], ...mArr].join('').split(' ').join(''));
        }
    }

    for (let [index, item] of sArr.entries()) {
        generateShortes(index);
    }

    rel.sort((a, b) => a.length - b.length);

    return rel[0];
}

/**
 * 给定一个整数n，找到最接近的整数（不包括它自己），这是一个回文
 * “最接近”被定义为两个整数之间的绝对差异最小化
 * 例如：
 *   输入："1222" 输出： "1221"
 *   输入："977" 输出： "979"
 *
 * @param { String } n
 * @return { String }
 */
function nearestPalindromic(n) {
    const nArr = n.split('');
    const prr = n.split('');
    const sArr = [];
    let rel = [];

    if (prr.length % 2) {
        prr[(prr.length - 1)/2] = parseInt(prr[(prr.length - 1)/2]) + 1;
        rel.push(prr.join(''));
    }

    if (!(nArr.length % 2)) nArr.splice((nArr.length) / 2, 0, '');

    let cl = nArr.slice(0, (nArr.length - 1) / 2);
    let cr = nArr.slice((nArr.length - 1) / 2 + 1).reverse();

    if (cl.join('') == cr.join('')) {
        const _ccl = cl.reverse();
        const _ccr = cr.reverse();
        const _cc = nArr[(nArr.length - 1) / 2];
        const _cl = Object.assign([], cl);
        const _we = [nArr[(nArr.length - 1) / 2], ..._ccl];

        for (let i = 0; i < _we.length; i++) {
            if (i == _we.length - 1) {
                if (_we[i] > 1) {
                    _ccl[i - 1] = _we[i] - 1;
                    _ccr[i - 1] = _we[i] - 1;
                } else {
                    let _str = '';
                    for (let j = 0; j < n.length - 1; j++) {
                        _str += '9';
                    }

                    rel.push(_str);
                }
            }

            if (_we[i] > 0 && (i < _we.length - 1)) {
                if (i == 0) {
                    nArr[(nArr.length - 1) / 2] = _we[i] - 1;
                } else {
                    _ccl[i - 1] = _we[i] - 1;
                    _ccr[i - 1] = _we[i] - 1;
                }

                break;
            }
        }

        cl = _ccl.reverse();
        cr = _ccr.reverse();
    }

    let wq = '';
    let wqr = [];

    for (let i = 0; i < n.length - 1; i++) {
        wq += '9';
    }
    rel.push(wq);

    for (let i = 0; i < n.length + 1; i++) {
        wqr.push('0');
    }

    wqr[0] = '1';
    wqr[wqr.length - 1] = '1';
    rel.push(wqr.join(''));

    for (let [index, item] of cl.entries()) {
        sArr.push([item, cr[index]]);
    }

    function joinBreak(ci, s = '') {
        if (ci == sArr.length - 1) {
            for (let [index, item] of sArr[ci].entries()) {
                const _st = s + item;
                const sdd = _st + nArr[(nArr.length - 1) / 2] + _st.split('').reverse().join('');

                if (sdd != n) {
                    rel.push(_st + nArr[(nArr.length - 1) / 2] + _st.split('').reverse().join(''));
                }
            }

            return;
        }

        for (let [index, item] of sArr[ci].entries()) {
            joinBreak(ci + 1, s + item);
        }
    }

    joinBreak(0);

    let arr = [];
    let rrr = [];

    for (let i = 0; i < rel.length; i++) {
        arr.push(Math.abs(parseInt(rel[i]) - parseInt(n)));
    }

    const min = Math.min(...arr);

    for (let i = 0; i < arr.length; i++) {
        if (arr[i] == min) {
            rrr.push(rel[i]);
        }
    }

    rrr.sort((a, b) => a - b);

    return rrr[0];
}

/**
 * 给定一个数组nums，有一个大小为k的滑动窗口，它从数组的最左边移动到最右边
 * 您只能在窗口中看到k个数字。每次滑动窗口向右移动一个位置
 * 您的工作是输出原始数组中每个窗口的中位数组
 * 例如：
 *   给定nums = [1, 3, -1, -3, 5, 3, 6, 7]，并且k = 3。
 *   窗口位置中位数
 *   ---------------------
 *   [1 3 -1] -3 5 3 6 7 1
 *   1 [3 -1 -3] 5 3 6 7 -1
 *   1 3 [-1 -3 5] 3 6 7 -1
 *   1 3 -1 [-3 5 3] 6 7 3
 *   1 3 -1 -3 [5 3 6] 7 5
 *   1 3 -1 -3 5 [3 6 7] 6
 *   ---------------------
 *   返回中值滑动窗口[1, -1, -1, 3, 5, 6]。
 *
 * @param { Array } nums
 * @param { Number } k
 * @return { Array }
 */
function medianSlidingWindow(nums, k) {
    const nArr = Object.assign([], nums);
    const sArr = [];
    const rel = [];

    for (let [index, item] of nArr.entries()) {
        if (index + k <= nArr.length) {
            sArr.push(nArr.slice(index, index + k));
        }
    }

    for (let [index, item] of sArr.entries()) {
        rel.push(getMedianNum(item));
    }

    function getMedianNum(arg) {
        arg.sort((a, b) => a - b);

        if (arg.length % 2 == 0) {
            return parseFloat((arg[arg.length / 2] + arg[arg.length / 2 - 1]) / 2);
        } else {
            return arg[parseInt(arg.length / 2)];
        }
    }

    return rel;
}

/**
 * 给定一个文件的绝对路径，简化它。
 * 例如，
 *  path = "/home/"，=> "/home"
 *  path = "/a/./b/../../c/"，=>"/c"
 *
 * @param { String } path
 * @return { String }
 */
function simplifyPath(path) {
    path = path.replace(/(\/\.\/)/g, '/');
    path = path.replace(/(\/\/)/g, '/');

    const pArr = [];
    const sArr= path.split('/');
    let rel = '';

    for (let item of sArr) {
        if (!item) continue;
        if (item == '..') {
            pArr.pop();
        } else {
            pArr.push(item);
        }
    }

    for (let item of pArr) {
        rel += '/' + item;
    }

    return rel;
}

/**
 * 鉴于仅包含数字的字符串0-9和目标值，返回所有的可能性
 * 二进制运算符（不操作符）+，-或*之间的数字，使他们和目标值相等
 *
 * 例 1:
 * Input: num = "123", target = 6
 * Output: ["1+2+3", "1*2*3"]
 *
 * 例 2:
 * Input: num = "232", target = 8
 * Output: ["2*3+2", "2+3*2"]
 *
 * @param { String } num
 * @param { Number } target
 * @return { Array }
 */
function addOperators(num, target) {
    const clr = ['+', '-', '*'];
    const nArr = num.split('');
    const sArr = [];
    const cArr = [];
    const rel = [];

    function setSplitItem(k, n) {
        let _spr = [];

        function splitItem(sr = '', k, n) {
            const _cArr = sr ? sr.split('|') : [];

            if (_cArr.length == n) {
                _cArr.sort((a, b) => a - b);

                if (_spr.indexOf(_cArr.join('|')) < 0) {
                    _spr.push(_cArr.join('|'));
                }

                return;
            }

            for (let i = 1; i <= k; i++) {
                if (_cArr.indexOf(String(i)) < 0) {
                    let _cr = sr;

                    if (_cArr.length > 0) {
                        _cr = _cr + '|' + i;
                    } else {
                        _cr += i;
                    }

                    splitItem(_cr, k, n);
                }
            }
        }

        splitItem('', k, n);

        for (let item of _spr) {
            const cir = item.split('|');
            let cs = '';

            for (let [index, cItem] of cir.entries()) {
                if (index == 0) {
                    cs += num.slice(0, cItem);
                } else {
                    cs += '|' + num.slice(cir[index - 1], cItem);
                }
            }

            cs += '|' + num.slice(cir[cir.length - 1]);

            sArr.push(cs.split('|'));
        }
    }

    function setCacular(ns, cx, sp = '') {
        const _pr = ns.split('|');
        sp += _pr[cx];

        if (cx == _pr.length - 1) {
            cArr.push(sp);

            return
        }

        for (let item of clr) {
            setCacular(ns, cx + 1, sp + item);
        }
    }

    function evil(fn) {
        const Fn = Function;

        return new Fn('return ' + fn)();
    }

    for (let i = 2; i <= nArr.length; i++) {
        setSplitItem(nArr.length - 1, i - 1);
    }

    for (let i = 0; i < sArr.length; i++) {
        setCacular(sArr[i].join('|'), 0, '');
    }

    for (let item of cArr) {
        if (evil(item) == target) {
            rel.push(item);
        }
    }

    return rel;
}

/**
 * 给定一个填充了0和1的二进制矩阵，找到只包含1的最大矩形并返回其面积
 * 例：
 * 输入：
 * [
 *   ['1', '0', '1', '0', '0']，
 *   ['1', '0', '1', '1', '0']，
 *   ['1', '1', '1', '1', '1']，
 *   ['1', '0', '0', '1', '0']
 * ]
 * 输出： 6
 *
 * @param { Array } matrix
 * @return { Number }
 */
function maximalRectangle(matrix) {
    const sArr = [];
    const rel = [];

    for (let [ci, cItem] of matrix.entries()) {
        for (let [mi, mItem] of cItem.entries()) {
            if (mItem == '1') {
                sArr.push({
                    c: ci,
                    m: mi,
                })
            }
        }
    }

    for (let item of sArr) {
        rel.push(getMaxOrdinate(item.c, item.m));
    }

    function getMaxOrdinate(c, m) {
        const _oArr = [];
        let _crs = 1;

        for (let i = 1;;i++) {
            if (!matrix[c][m + i] || matrix[c][m + i] == '0') {
                _crs = i;

                break;
            }
        }

        for (let j = 1; j <= _crs; j++) {
            _oArr.push(getRectArea(c, m, j) * j);
        }

        function getRectArea(c, m, l) {
            let _l = 1;

            for (let i = 1;;i++) {
                let pFlag = true;

                if (matrix[c + i]) {
                    for (let j = 0; j < l; j++) {
                        if ((matrix[c + i][m + j]) == 0) {
                            pFlag = false;
                        }
                    }

                    if (!pFlag) {
                        _l = i;

                        break;
                    }
                } else {
                    _l = i;

                    break;
                }
            }

            return _l;
        }

        return Math.max(..._oArr);
    }

    return rel.length ? Math.max(...rel) : 0;
}

/**
 * 给定一组候选号码（candidates）（没有重复）和目标号码（target），找出candidates 候选号码总和的所有唯一组合target。
 * 相同重复数目可以选自candidates 无限次数。
 * 例1：
 * 输入： candidates = [2,3,6,7], target = 7，
 * 结果：
 * [
 * [7]，
 * [2,2,3]
 * ]
 *
 * @param { Array } candidates
 * @param { Number } target
 * @return { Array }
 */
function combinationSum(candidates, target) {
    const tArr = [];
    const sArr = new Set();
    const rel = [];

    for (let i = 0; i < target; i++) {
        tArr.push('*');
    }

    function setSplitItem(k, n) {
        let _spr = [];

        function splitItem(sr = '', k, n) {
            const _cArr = sr ? sr.split('|') : [];

            if (_cArr.length == n) {
                _cArr.sort((a, b) => a - b);

                if (_spr.indexOf(_cArr.join('|')) < 0) {
                    _spr.push(_cArr.join('|'));
                }

                return;
            }

            for (let i = 1; i <= k; i++) {
                if (_cArr.indexOf(String(i)) < 0) {
                    let _cr = sr;

                    if (_cArr.length > 0) {
                        _cr = _cr + '|' + i;
                    } else {
                        _cr += i;
                    }

                    splitItem(_cr, k, n);
                }
            }
        }

        splitItem('', k, n);

        for (let item of _spr) {
            const cir = item.split('|');
            const _csr = [];
            let cs = '';

            for (let [index, cItem] of cir.entries()) {
                if (index == 0) {
                    cs += tArr.slice(0, cItem);
                } else {
                    cs += '|' + tArr.slice(cir[index - 1], cItem);
                }
            }

            cs += '|' + tArr.slice(cir[cir.length - 1]);

            for (let cItem of cs.split('|')) {
                _csr.push(cItem.split(',').join('').length);
            }

            _csr.sort((a, b) => a - b);

            sArr.add(_csr.join(','));
        }
    }

    for (let i = 2; i <= target; i++) {
        setSplitItem(target - 1, i - 1);
    }

    for (let item1 of sArr) {
        let isConfirm = true;

        for (let item2 of item1.split(',')) {
            if (candidates.indexOf(parseInt(item2)) < 0) {
                isConfirm = false;
            }
        }

        if (isConfirm) {
            let fArr = item1.split(',');

            for (let i = 0; i < fArr.length; i++) {
                fArr[i] = parseInt(fArr[i]);
            }

            rel.push(fArr);
        }
    }

    if (candidates.indexOf(target) > -1) {
        rel.push([target]);
    }

    return rel;
}

/**
 * 将非负整数转换为其对应的中文繁体表示。可以保证给定输入小于 231 - 1
 * 例：
 * 输入：
 * 100860
 * 输出： 壹拾万零捌佰陆拾
 *
 * @param { Number } num
 * @return { String }
 */
function numberToWords(num) {
    let numStr = '' + num;
    const tArr = [
        { a: 0, c: '零' },
        { a: 1, c: '壹' },
        { a: 2, c: '贰' },
        { a: 3, c: '叁' },
        { a: 4, c: '肆' },
        { a: 5, c: '伍' },
        { a: 6, c: '陆' },
        { a: 7, c: '柒' },
        { a: 8, c: '捌' },
        { a: 9, c: '九' },
    ];
    const sArr = ['拾', '佰', '仟', '万', '万拾', '万佰', '万仟', '亿', '亿拾', '亿佰', '亿仟', '亿万'];

    String.prototype.replaceAll = function (s1, s2) {
        return this.replace(new RegExp(s1, "gm"), s2);
    };

    for (let item of tArr) {
        numStr = numStr.replaceAll(item.a, item.c);
    }

    numStr = numStr.split('').reverse().join('');

    let sl = '';
    for (let [index, item] of Array.from(numStr).entries()) {
        if (index === 0) {
            if (item !== '零') {
                sl += item;
            }
        } else {
            if (item === '零') {
                sl += '零';
            } else {
                if (sArr[index - 1].length > 1) {

                    if (index <= 7) {
                        if (sl.indexOf('万') > -1) {
                            sl = sl + sArr[index - 1][1] + item;
                        } else {
                            sl = sl + sArr[index - 1] + item;
                        }
                    } else if (index > 7) {
                        if (sl.indexOf('亿') > -1) {
                            sl = sl + sArr[index - 1][1] + item;
                        } else {
                            sl = sl + sArr[index - 1] + item;
                        }
                    }

                } else {
                    sl = sl + sArr[index - 1] + item;
                }
            }
        }
    }

  let rel = sl.split('').reverse().join('');
  rel = rel.replaceAll(/零{1,}/g, '零');

  return rel;
}

```
