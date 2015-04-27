---
layout: post
title:  "sublime-jekyll 플러그인 "
date:   2015-04-26 18:00:00
categories: jekyll update
---

블로그에 글을 올릴 때 에디터로 Sublime Text를 사용하고 있다. jekyll 플러그인이 있어 사용해보았다. 

## sublime-jekyll 플러그인 

# Install (OSX) 하기

1. PackageControl을 연다. command + shift + p (Tools > Commmand Palette menu) 
2. Package Control : Install Package 선택 
3. Package Control이 없다면 Package Control부터 설치한다.  
 - [Install PackageControl] 에서 사용 중인 버전 (Sublime Text 2 Or Sublime Text3) 스크립트 복사 
 - View > Show Console(Ctrl + ') 복사한 스크립트를 실행하 Package Control 이 설치된다.   

4. Install Pacakcge 를 선택하면 나오는 검색 창에서 jekyll로 검색하여 나오는 jekyll을 설치한다. 

# 설정하기 

1. Preference > Pacakge Settings > Jekyll > Settings - Users

2. posts_path, drafts_path를 설정한다. 
그외 default 설정이나 날짜 포맷 등을 필요에 따라 설정 할 수 있다. 

{% highlight text %}

{

    // This should point to your "_posts" directory.
    // NOTE: This should be an absolute path. Also, the path should
    // match your system convention. For example, Windows machines should
    // have a path similar to "C:\\Users\\username\\site\\_posts".
    // *nix systems should have a path similar to "/Users/username/site/_posts".
    "posts_path": "/Users/nhnent/iyoon.github.com/_posts",

    // This should point to your "_drafts" directory.
    // NOTE: This should be an absolute path. Also, the path should
    // match your system convention. For example, Windows machines should
    // have a path similar to "C:\\Users\\username\\site\\_drafts".
    // *nix systems should have a path similar to "/Users/username/site/_drafts".
    "drafts_path": "/Users/nhnent/iyoon.github.com/_drafts",

    // If you have multiple Jekyll blogs, but don't use Sumblime Projects,
    // you can optionally have sublime-jekyll look for the `_posts` or `_drafts`
    // folders open in your sidebar. This should have a value of true or false.
    "automatically_find_paths": false,

    // This string value should represent the default syntax for a new post.
    // Valid options are: "Markdown", "Textile"
    "default_post_syntax": "Markdown",

    /** *****************************************************************************
     * Post Front-matter Defaults
     *
     * Set these values to make your life easier when composing new posts. This is
     * similar to setting your defaults as part of the `_config.yml` file:
     *
     * http://jekyllrb.com/docs/configuration/#front-matter-defaults
     * ******************************************************************************
     */

    // This string value should represent the default layout for new posts.
    "default_post_layout": "",

    // This value should represent the default categories for new posts.
    // Each category should be entered as a list item in string format
    // with commas separating values ["cat1", "cat2"]. To remove this key
    // from your front-matter completely, pass a value of `null`.
    "default_post_categories": [],

    // This value should represent the default tags for new posts.
    // Each tag should be entered as a list item in string format
    // with commas separating values ["tag1", "tag2"]. To remove this key
    // from your front-matter completely, pass a value of `null`.
    "default_post_tags": [],

    // A boolean specifying if you want new posts to be marked as published.
    // To remove this key from your front-matter completely, pass a value of `null`.
    "default_post_published": true,

    // If you need to add additional front-matter `key: value` information to
    // your posts, you can store them in a dictionary object using a format
    // like {"foo": "bar", "baz": "qux"}. This dictionary will be appended to
    // any of the enabled default keys above (Reminder: the `title` and `layout`
    // keys will always be included, so **DO NOT** include them in the extras dictionary).
    "default_post_extras": {},

    /** ***********************************************************************************
     * If for some reason you want to change the way either the date
     * or the datetime string is formatted, you can override those formats
     * here using valid Python datetime.strftime() format codes.
     *
     * If you need a refresher on these codes, have a look at the Python
     * documentation found here:
     *
     * http://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior
     * ******************************************************************************
     */

    // A valid Python strftime string
    "insert_date_format": "%Y-%m-%d",

    // A valid Python strftime string
    "insert_datetime_format": "%Y-%m-%d %H:%M:%S"

}

{% endhighlight %}


# 기능 

Command + shift + p 에서 jekyll 플러그인의 기능을 사용 할 수 있다. 

- Jekyll: New post 
- Jekyll: New draft 
- Jekyll: Open post
- Jekyll: Open draft
- Jekyll: Insert current date 
- Jekyll: Insert current datetime 
- Jekyll: Promote draft to post

# Syntax 강조 
Set Syntax:Markdown(Jekyll)

# 자동완성 

{% raw %}
축약형을 입력한 후 Tab을 누르면 자동완성이 된다.  
hightlight + Tab = {% highlight %}{% endhighlight %} 

- assign: {% assign a = b %}
- capture: {% capture %}{% endcapture %}
- case: {% case %}{% endcase %}
- comment: {% comment %}{% endcomment %}



{% endraw %}



https://packagecontrol.io/packages/Jekyll Snippets 


> 참고자료  
[sublime-jekyll][sublime-jekyll]   
[Install PackageControl] [Install PackageControl]  

[Install PackageControl]: https://packagecontrol.io/installation

[sublime-jekyll]: https://packagecontrol.io/packages/Jekyll

