# Spotify Data Analysis: What Characteristics Make Songs Reach the Top 100?

Samantha Webster - swebster@chapman.edu
Allissa Caltagirone - calta102@mail.chapman.edu
Yaxi Lei - ylei@chapman.edu


You can use the [editor on GitHub](https://github.com/swebster2/Spotify-Data-Analysis/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Code

```{}
setwd("/Users/1d_lyx/Desktop/Rdataset/")
songDF <- read.csv("featuresdf.csv")
songDF2 <- read.csv("data.csv")
songDF2$key <- as.factor(songDF2$key)
songDF2$mode <- as.factor(songDF2$mode)
songDF2$time_signature <- as.factor(songDF2$time_signature)
songDF2$target <- as.factor(songDF2$target)
```

```{}
songDF$mode <- as.factor(songDF$mode)
summary(songDF[,-c(1,2)]) #min, max, and mean
round(sapply(songDF[,-c(1,2)], sd), 4) #standard deviation
```
We delete the first two rows because they are id and names, which does not make sense in this analysis.

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/swebster2/Spotify-Data-Analysis/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
