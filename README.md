以下是为您创建的Google Chrome扩展，它会在打开新标签页时自动跳转到您设置的主页网站：

扩展文件结构
homepage-newtab/
├── manifest.json
├── newtab.html
├── newtab.js
├── options.html
├── options.js
└── icon128.png (需要自行准备图标文件)
1. manifest.json
json
{
    "manifest_version": 3,
    "name": "主页新标签页",
    "version": "1.0",
    "description": "将新标签页重定向到自定义主页",
    "chrome_url_overrides": {
        "newtab": "newtab.html"
    },
    "permissions": ["storage"],
    "options_page": "options.html",
    "icons": {
        "128": "icon128.png"
    }
}
2. newtab.html
html
<!DOCTYPE html>
<html>
<head>
    <title>主页新标签页</title>
    <script src="newtab.js"></script>
</head>
<body>
</body>
</html>
3. newtab.js
javascript
// 获取存储的主页地址并重定向
chrome.storage.sync.get(['homepage'], (data) => {
    const defaultHomepage = 'https://www.google.com';
    const targetURL = data.homepage || defaultHomepage;
    window.location.replace(targetURL);
});
4. options.html (设置页面)
html
<!DOCTYPE html>
<html>
<head>
    <title>设置主页地址</title>
    <style>
        body { padding: 20px; font-family: Arial, sans-serif; }
        .container { max-width: 600px; margin: 0 auto; }
        input[type="url"] { width: 100%; padding: 8px; margin: 10px 0; }
        button { background: #0078d4; color: white; border: none; padding: 10px 20px; cursor: pointer; }
    </style>
</head>
<body>
    <div class="container">
        <h2>设置您的主页地址</h2>
        <input type="url" id="homepage" placeholder="https://www.example.com">
        <button id="save">保存设置</button>
    </div>
    <script src="options.js"></script>
</body>
</html>
5. options.js
javascript
document.addEventListener('DOMContentLoaded', () => {
    // 加载已保存的设置
    chrome.storage.sync.get(['homepage'], (data) => {
        document.getElementById('homepage').value = data.homepage || '';
    });

    // 保存设置
    document.getElementById('save').addEventListener('click', () => {
        const homepage = document.getElementById('homepage').value;
        if (homepage) {
            chrome.storage.sync.set({ homepage }, () => {
                alert('设置已保存！');
            });
        }
    });
});
使用说明
准备一个128x128像素的PNG图标命名为icon128.png

在Chrome浏览器中访问：

chrome://extensions
开启右上角的"开发者模式"

点击"加载已解压的扩展程序"，选择整个文件夹

点击扩展图标，选择"选项"进行主页设置

注意事项
默认主页为Google，可在设置中更改为任意合法URL
不支持chrome://开头的特殊地址
如果跳转失败会停留在空白页，请检查输入的URL格式
支持Chrome同步功能，设置会自动同步到登录的Google账号
这个扩展通过覆盖新标签页的方式实现主页跳转功能，同时提供了友好的设置界面，用户可以根据需要随时修改目标主页地址。

