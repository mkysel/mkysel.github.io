---
title: "From Ugly to Beautiful: The Transformation of Martinkysel.com"
date: "2023-02-25"
categories: 
  - "migration"
excerpt: Migrating a website from one platform to another can be a daunting task, but with the right tools and techniques, it doesn't have to be a nightmare. In my case, I recently migrated my blog from WordPress to Jekyll, and I'm happy to report that the process was relatively smooth and painless.
---

Migrating a website from one platform to another can be a daunting task, but with the right tools and techniques, it doesn't have to be a nightmare. In my case, I recently migrated my blog from WordPress to Jekyll, and I'm happy to report that the process was relatively smooth and painless.

## The Initial Problem: Hosting on a Basement Server

When I first started my blog on WordPress, I had a rather unusual setup - the site was hosted on a server in a friend's basement. While it may sound like a bit of a "ghetto" arrangement, it seemed like a cost-effective solution at the time. However, it soon became apparent that this setup came with its fair share of problems.

One of the biggest issues was the server's reliability. For reasons that were never quite clear, the server seemed to go down regularly, without any good explanation. This meant that my site was often inaccessible for hours or even days at a time, frustrating both myself and my readers.

To make matters worse, the site's loading times were notoriously slow. This was likely due in part to the server's less-than-optimal location and setup, but it also meant that visitors to my site had to endure lengthy wait times just to access my content. And as we all know, in the world of online content, every second counts.

As my blog began to grow and attract more readers, the maintenance of the site became an increasingly daunting task. With the server regularly going down and loading times being so slow, I found myself spending more and more time just trying to keep the site running smoothly. This detracted from the time and energy I wanted to devote to creating new content and engaging with my readers.

Eventually, in 2020, I began to investigate the possibility of moving my site to a proper hosting solution, such as AWS. However, as I quickly discovered, this option came with a hefty price tag. The cost of a properly hosted server, combined with the need for a distributed database, made this option simply unfeasible for me at the time.

It's not just about hosting, maintaining WordPress can be a headache. While it's an amazing system, opting for a static website can offer several advantages. It's faster, easier to maintain, and more secure, making it a smart choice for those who value efficiency and security.

In the end, I realized that something had to be done to address the ongoing issues with my site's performance and reliability. After careful consideration and research, I made the decision to migrate my site to Jekyll, a static site generator that promised faster load times and a more streamlined maintenance process. And while the process of migrating my site was not without its challenges, I can now say that I am thrilled with the improved performance and ease of use that Jekyll has brought to my blog.

I chose GitHub Pages as my hosting solution, as I regularly use Git and GitHub in my daily work, and the added bonus of open-sourcing the content was a nice benefit for the community.

## The Long and Winding Road of Migrating to Markdown - According to the Internet

When I stumbled upon Swizec's [article](https://swizec.com/blog/how-to-export-a-large-wordpress-site-to-markdown/) on migrating from WordPress to Markdown, I was initially excited. His straightforward breakdown of the process made it seem like a simple task that could be completed in an afternoon. "Pfft, an afternoon of work at worst," he wrote, listing off the steps like they were nothing: export from WordPress, find a script, sip margaritas.

It all sounded so easy and straightforward, and I couldn't help but agree with Swizec's assessment. "That sounds about right," I thought to myself, feeling confident that I could tackle this migration with ease.

But then, Swizec hit me with a dose of reality. "Suddenly it's 6 months later and you're losing your mind," he wrote. His words were a stark reminder that even the simplest tasks can often become complex and time-consuming.

As an optimist, however, I refused to be discouraged. Sure, the process might not be as straightforward as I initially thought, but surely it couldn't be that difficult, right?

So, being the lazy optimist that I am, I decided to explore my options. I turned to Fiverr, hoping to find someone who could handle the migration for me. I was shocked to discover that most of the quotes I received ranged from $1500 to $8000. That seemed like an exorbitant amount of money for something that sounded like it should only take a few hours.

One freelancer even warned me that it could take 2-3 months to complete the migration, although he optimistically suggested that it might only take a month.

The whole experience left me feeling a bit frustrated and overwhelmed. But despite the initial setbacks, I remained determined to find a solution that would allow me to migrate my site without breaking the bank or losing my mind in the process. And while it might take a bit more time and effort than I initially anticipated, I'm confident that I'll be able to find a way to make it work.

## What Made This Blog Different

Perhaps I was a bit fortunate that I had recently teamed up with [Juraj](https://github.com/spello2287) to create a brand new website for [Nomadic Workplace](https://nomadicworkplace.com/), my umbrella company that enables me to pursue my digital nomad lifestyle while remaining legally employed in the US.

When it comes to martinkysel.com, I can confidently say that it boasts two significant advantages over other blogs out there. Firstly, the vast majority of its content follows a similar pattern and covers similar topics. This consistency makes it easier to script a migration without having to deal with a lot of edge cases.

Secondly, all of the comments on the blog are saved in Disqus. This means that they can be easily transferred and integrated into a new platform, without the risk of losing valuable feedback and engagement from readers.

Finally, let's be honest - the old blog design was pretty unappealing. So, any improvement in this area would be a massive gain for both myself and my readers. With the new Jekyll-based site, I was able to create a much cleaner, more streamlined design that not only looks better but also makes it easier for readers to navigate and engage with the content.

## The Steps of the Actual Migration Process

So, how did the whole migration process actually go? Well, to start with, I discovered a migration tool developed by Lonekorean over on [Github](https://github.com/lonekorean/wordpress-export-to-markdown), which was almost perfect for my needs. However, there were a couple of essential components that it was missing.

Firstly, it didn't have the ability to deal with `[code]` blocks, which seemed to be a non-standard feature of my old Wordpress site. This was a bit of a headache, but as is often the case with internet problems, I was able to find someone who had already struggled with and solved the issue in the past. In this case, it was a user called mxro, who had written some incredibly simple [Typescript](https://github.com/lonekorean/wordpress-export-to-markdown/issues/86#issuecomment-1304870098) code to handle the preprocessing.

The second issue I encountered was with images. Thankfully, there were only a few scattered throughout the entire blog, so I was able to handle them all manually. While this was a bit time-consuming, it ultimately wasn't a huge deal in the grand scheme of things.

Of course, the migration process wasn't just about transferring content from one platform to another. I also had to select a new theme for the site, add some new icons, turn the banner into an avatar, and reset DNS entries. While these tasks weren't necessarily difficult, they did require some careful attention to detail in order to ensure that everything was working smoothly and looking great.

## A Surprisingly Short Migration Process

The entire process took only *four hours* - no joke. I started around 8PM and got so absorbed in it that I barely noticed the time passing. I remember making a fresh batch of argentinean mate while working away. By around 11PM, I decided the website was good enough and made the move to switch the DNS entries over to the new website. Of course, there may be some kinks to iron out over the next few days as I discover any issues that may have gone wrong or missing, but overall, it was nowhere near the month or six months I was anticipating.
And if you don't believe me, you can check the git history.
The blog's source is open on [GitHub Pages](https://github.com/mkysel/mkysel.github.io).

## Lessons Learned: Be Skeptical

Reflecting on this experience, I have come to realize that there are a few key takeaways that I believe are worth sharing with others.

Firstly, when it comes to using standard tools, the integration with existing systems can be seamless. In my case, because I was using widely accepted tools for the migration process, such as WordPress and Markdown, the transition was relatively smooth. However, the process could have been complicated if I had relied on more obscure or specialized tools.

Secondly, keep it simple stupid. Pick a system that requires the least maintenance.

Thirdly, it is important to approach time estimates with a healthy dose of skepticism. I was initially quoted a range of 1-3 months for the migration process by one service provider, which turned out to be completely off-base. It is essential to use common sense and T-shirt sizing estimates.

Lastly, be wary if your customer is another software engineer. Trying to "bullshit" your way through a negotiation will only lead to frustration and disappointment for both parties. Other engineers are highly skilled professionals who understand the complexities of building and maintaining software systems. They know what is feasible and what is not, and inaccurate estimates should alarm them to sketchy behavior.
