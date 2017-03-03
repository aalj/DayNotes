1. WebViewClient 帮助WebView处理各种通知，请求事件

2. WebChromeClient 辅助WebView处理Javascript的对话框，网站图标，网站title ，加载进度等

   简单的使用实例

   ```java
   WebView webView;
   webView= (WebView) findViewById(R.id.webview);
   webView.setWebChromeClient(new WebChromeClient());
   webView.setWebViewClient(new WebViewClient());
   webView.getSettings().setJavaScriptEnabled(true);
   webView.loadUrl(url);
   ```

   ​

