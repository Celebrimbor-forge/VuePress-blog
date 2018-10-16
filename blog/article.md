---
title: Making a simple blog using VuePress
date: 2018-10-16
description:
    An article I wrote to Medium. It offers a basic tutorial on how to use VuePress to make a simple blog.
---

# Making a simple blog using VuePress

---
## Table of contents

[[toc]]

Making a simple blog using VuePress
I've always had problems adopting new technologies and frameworks without seeing a simple tutorial of the basics. Now that I'm slowly working with more and more open source projects I thought it was about time I started giving back to the community.

I've gotten extremely frustrated with poor documentation and I'm sure most developers are in the same boat with me. Poor documentation is a plague and it spreads because developers have deadlines they have to make and usually projects have problems and the deadline can already be seen around the corner. Solution? Cut testing! As long as our code is bug free it's just wasted time right? Often this is still not enought and the developer would rather use the time for something else. So the solution becomes: Rush through writing documentation.

Vue.js is one of the best documentated frameworks I have ever seen…this might be one of the main reasons I love using it. How do I get the same kind of documentation that Vue.js has, you might ask. I'd start with using the same tools, VuePress. VuePress was made by the creator of Vue.js, Evan You to make the documentation of Vue.js less painful.


---

## Getting started
Things needed before starting:

- Node package manager (NPM)
- Basic understanding of Vue.js
- Knowing how to open the terminal/CMD
- A proper text editor helps

## Installing VuePress
You have your NPM installed and your terminal open? Great! All you need to do to install VuePress is a simple:
npm i -g vuepress

## Making the site
Basically you only need a README.md file to use VuePress. You can either make your own folder with a README.md file or clone the repository I made for this tutorial.
There are two easy commands to run VuePress from the command line:
vuepress dev is used to start a development server that loads your changes automatically and all the other dev goodies you'd need. This will be the command you'll be using the most.
vuepress build is used for, surprise, building your website. It'll basically generate all the assets you need to deploy your site to a hosting service.

Add some text to your README.md . I personally like to use this amazing site that does lorem ipsum the Samuel L. Jackson way…just make sure to replace that before sending it to a client. Example: 

```
## Samuel L. Ipsum
The path of the righteous man is beset on all sides by the iniquities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children. And I will strike down upon thee with great vengeance and furious anger those who would attempt to poison and destroy My brothers. And you will know My name is the Lord when I lay My vengeance upon thee.
```

Once you have your README.md full of text then you can run vuepress dev then open up http://localhost:8080 on your browser and see the extremely clean looking default theme for VuePress.
You should be seeing thisThat was easy, but you'd probably like to have something more than just a single page of plain text right? Let's do just that!

## Adding a title
First we need to create a folder called .vuepress and add a configuration file named config.js . This file is used for customizing the website. It's a JavaScript file that should export a JSON object. There are plenty of stuff you can change with the config.js, but we will just focus on a few so you get a basic idea on what you can do with VuePress. For full set of arguments, check the documentation.
Let's add a title:

```
// .vuepress/config.js
module.exports = {
    title: 'VuePress tutorial'
}
```

You should see the title appear in the left side of the navbar. Easy right?

## Making a list of blog posts
Now let's add a blog! With custom Vue components, adding a blog is pretty easy. Let's make a custom Vue component that renders a list of blog posts.
Create a new folder under .vuepressand name it components. Under the components folder we'll add our custom Vue components and then VuePress will automatically make them available globally, no need to import.
This is the code for the component, which is named BlogIndex.vue:

```
<!-- /.vuepress/components/BlogIndex.vue -->

<template>
<div>
    <div v-for="post in posts">
        <h2>
            <router-link :to="post.path">{{ post.frontmatter.title }}</router-link>
        </h2>
        
        <p>{{ post.frontmatter.description }}</p>

        <p><router-link :to="post.path">Continue reading&hellip;</router-link></p>
    </div>
</div>
</template>

<script>
export default {
    computed: {
        posts() {
            return this.$site.pages
                .filter(x => x.path.startsWith('/blog/') && !x.frontmatter.blog_index)
                .sort((a, b) => new Date(b.frontmatter.date) - new Date(a.frontmatter.date));
        }
    }
}
</script>
```

That's the code. Now let's take a closer look on what's happening in there!
The template is fairly basic. The VuePress components use a single-file component pattern.
Let's start from the top. We have the html-template that renders posts in the computed property named posts with the v-for loop. The computed posts() gets all the blog posts from the site pages. The blog posts will have some required front matter information, like a title. The date shall also be required so we can sort the posts and have the newest one appear first.
Another noteworthy thing are the two properties that VuePress comes ready with: this.$site which gives you meta data of the website (each page included) and this.$page which gives you information about the current page.
The component is ready, now we'll add some content!

## Content to the blog!
Make a folder to the root of the project named blog and in that folder add a new README.md file. This file will serve as the index to our blog and we will include our BlogIndex.vue component here.

```
---
blog_index: true
---

# Blog
This will be our blog. Welcome.

<BlogIndex />
```

The code in ./blog/README.md has the front matter of blog_index: true so it won't be showing in our list of posts.
Notice how easy it is to import custom components. VuePress registers it automatically and you can use it in any markdown file. A familiar syntax if you've been using Vue.js.
Next it's time to add the first blog post to our blog folder. Let's add a file to the blog folder named article.md this file will serve us as our first blog post.

```
<!-- ./blog/article.md --!>
---
title: Making a simple blog using VuePress
date: 2018–10–16
description:
   An article I wrote to Medium. It offers a basic tutorial on how       to use VuePress to make a simple blog.
---
# Making a simple blog using VuePress
/* I will add the full article here and in a future "VuePress styling" tutorial I will show how to style the VuePress page */
```

We've added title , date and description to the front matter (If you're confused with the term "front matter", this article explains it pretty well). Every blog post should have these defined, since they will be used when listing the blog.
Last thing we need to add is the link to the navbar. So open up your ./.vuepress/config.js and add the following:

```
// /.vuepress/config.js
module.exports = {
    title: 'VuePress tutorial',
    themeConfig: {
        nav: [
            { text: 'Home', link: '/' },
            { text: 'Blog', link: '/blog/' }
        ]
    }
}
```

That's pretty much it for a super basic blog. I will be making another tutorial in styling your VuePress blog.