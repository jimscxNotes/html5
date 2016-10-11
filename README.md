### html5 history
### HTML5 history两方法一事件
两种方法都允许我们添加和更新历史记录，它们的工作原理相同并且可以添加数量相同的参数。
除了方法之外，还有popstate事件。pushState()和replaceState()参数一样，参数说明如下：
```
1、state：存储JSON字符串，可以用在popstate事件中。
2、title：现在大多数浏览器不支持或者忽略这个参数，最好用null代替
3、url：任意有效的URL，用于更新浏览器的地址栏，并不在乎URL是否已经存在地址列表中。更重要的是，它不会重新加载页面.
```
#### 1. history.pushState();
pushState()是在history栈中添加一个新的条目；

#### 2. history.replaceState();
replaceState()是替换当前的记录值；

#### 3. popstate事件
不管你是点击前进或者后退按钮，还是使用history.go和history.back方法，popstate都会被触发。

#### html
```
<div class="container">
    <div class="row">
        <ul class="nav navbar-nav">
            <li><a href="home.html" class="historyAPI">Home</a></li>
            <li><a href="about.html" class="historyAPI">About</a></li>
            <li><a href="contact.html" class="historyAPI">Contact</a></li>
        </ul>
    </div>
    <div class="row">
        <div class="col-md-6">
            <div class="well">
                Click on Links above to see history API usage using <code>pushState</code> method.
            </div>
        </div>
        <div class="row">   
            <div class="jumbotron" id="contentHolder">
                <h1>Home!</h1>
                <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
            </div>
        </div>
    </div>
</div>
```
#### js:
```
<script>
    jQuery('document').ready(function(){

        jQuery('.historyAPI').on('click', function(e){
            e.preventDefault();
            var href = $(this).attr('href');

            // Getting Content
            getContent(href, true);

            jQuery('.historyAPI').parent().removeClass('active');
            $(this).parent().addClass('active');
        });

    });

    // Adding popstate event listener to handle browser back button  
    window.addEventListener("popstate", function(e) {

        // Update Content
        getContent(location.pathname, false);
        jQuery('.historyAPI').parent().removeClass('active');
        $('a[href="'+e.state.location+'"]').parent().addClass('active');

    });

    function getContent(url, addEntry) {
        $.get(url)
        .done(function( data ) {

            // Updating Content on Page
            $('#contentHolder').html(data);

            if(addEntry == true) {

                var stateData = {
                        "location": url 
                    }

                // Add History Entry using pushState
                history.pushState(stateData, null, url);	
            }


        });
    }
</script>	
```

