const axios = require('axios');
 
// 登录百度网盘，获取Cookies
async function loginToBaiduPan(username, password) {
    const loginUrl = 'https://pan.baidu.com/api/login';
    const loginData = {
        username: username,
        password: password
    };
 
    try {
        const response = await axios.post(loginUrl, loginData, { withCredentials: true });
        return response.headers['set-cookie'];
    } catch (error) {
        console.error('登录失败:', error);
        return null;
    }
}
 
// 使用Cookies请求百度网盘数据
async function fetchBaiduPanData(cookies) {
    const dataFetchUrl = 'https://pan.baidu.com/api/your-data-endpoint';
 
    try {
        const response = await axios.get(dataFetchUrl, {
            headers: { 'Cookie': cookies.join('; ') },
            withCredentials: true
        });
        return response.data;
    } catch (error) {
        console.error('数据请求失败:', error);
        return null;
    }
}
 
// 使用示例
async function main() {
    const username = '你的百度账号';
    const password = '你的百度密码';
    const cookies = await loginToBaiduPan(username, password);
    if (cookies) {
        const data = await fetchBaiduPanData(cookies);
        console.log(data);
    }
}
 
main();