# Iframe caused werid hehavior on browser history back button

## ❓ 🤷‍♂ What and how i met this

某天我接到个bug单，复现场景是这样的：

>通过菜单栏从页面A切换页面B，在页面B的结构是搜索栏➕查询结果的结构。
这时在页面点击查询后需要点击两次浏览器的返回键才能返回到页面A。

第一反应，这肯定是查询按钮的点击事件做了一些路由相关的操作，然后我顺着查询按钮去看它做了什么操作，然而并没有发现任何和路由相关的代码，很干净就只是触发了查询的Action，没有副作用
嗯? 然后，然后我就懵逼了...

## 🔨🧐Here we go
查询 点击 两次 返回 历史 路由...能想到的关键各种排列组合google一番后也没找到和我同样遭遇的。
这时我发现这个bug只会在页面A和B间出现，如果从页面A切换至页面C（C同样是查询栏➕结果的结构）在C点击查询后再点返回则是正常的。无头绪的经过一通瞎点后发现每多点一次查询，要回到A也同样需要多点一次返回。同时在和测试的沟通中得知这个bug在提单之前没有，并不是偶然发现的，而且很肯定，因为A和B之间互相切换是个业务上很常用的场景。啊哈～那看看最近页面A和B的git变动肯定能找到原因吧。
在一堆变更中，页面B一个新增的iframe成功引起了我的注意，src？--> history？在本地把iframe去掉后果然就好了，but y？

## 🙋‍♂️ 🎉 Uha! Got u
于是加上关键词**ifram**再次google，总算找到和我跳入同一个坑的了。
简单讲就是iframe的地址变更会通知给浏览器的导航加在历史记录里，在我的业务中新增的iframe的地址又是根据查询返回值来决定的，正是这样造成了我们不期待的影响。
我收藏起来的一些关于这个bug和iframe的解释：

* 这篇博客用一个简单的demo做了详细的复现和解释
  [Back Button Behavior on a Page With an iframe](http://www.webdeveasy.com/back-button-behavior-on-a-page-with-an-iframe/)
>I am developing a widget for websites. This widget lays inside an iframe in a website’s page. One of my users (which is a site owner) complained about a weird behavior of my widget. On pages where the widget was implemented, the browser’s back button didn’t work properly. Instead of navigating the user to the previous page on the website, the back button navigated the user to the previous page inside the iframe.

* [Rerendering components with iframe leads to browser history duplicates](https://github.com/ReactTraining/react-router/issues/6259)
> This is just how iframes work, unfortunately. The navigation events of the iframe (change its src attribute in this case) are propagated up to the parent window as well. So your duplication is just coming from the iframe itself.
Unfortunately, because CodeSandbox messes with history in its sandboxing, the issue is a bit obscured (removing the iframe results in no navigation events). But in a real world site, that's the behavior you'd see even without React Router.
by timdorr

* [Posting HTML form to iframe causes problems with browser history](https://stackoverflow.com/questions/37058852/posting-html-form-to-iframe-causes-problems-with-browser-history/37059485)
> However, I decided to remove the iframe from page-load, and instead dynamically create an iframe via JS when it was time to upload files. Once the files were uploaded and the iframe onload event fired, I then removed the iframe from the DOM via JS and it no longer caused the problem occurring above.
I'm honestly not too sure why that fixed the problem or if it's just a potential issue with browsers, but all the same, for anyone that wants to use an iframe to upload files on a form without reloading the page, be sure to not have the iframe on the page when it first loads, and instead dynamically add the iframe only when you need it and remove it when you're done.

* [react-router-and-iframe-browser-history](https://stackoverflow.com/questions/45277276/react-router-and-iframe-browser-history)
>Don't just change the src. You've to replace a new iframe everytime the src change. By doing that without change things much, just add difference key to your iframe everytime you change src
```react
render(){
const iframe = <iframe key={this.thing.id} src={this.thing.src} ></iframe>
return(iframe)
}
```

* 一些关于iframe的讨论
[3-reasons-why-you-should-not-use-iframes](https://enrega.com.au/websites/insights/3-reasons-why-you-should-not-use-iframes)
>Reason #2: iFrames cause usability issues
>The iFrame tag is notorious for creating usability annoyances. Among most common of them are:
```markdown
* it tends to break the "Back" button in the browser being used;
* it confuses visually impaired visitors, using screen readers;
* it confuses users, suddenly opening the iframe content in a new browser window;
* content within the iframe doesn't fit in and looks odd because it can ignore the styles sheets from within the main website;
* content within the iframe is missing since the source URL changed; and,
* navigation of the site in the iframe stops working.
```
>It is recommended to find better ways to refer your visitors to external content, or by serving the content from your own website, instead of placing it within an iFrame tag.


