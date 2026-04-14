# Blog Categories and Management

This guide explains how to manage blog categories while maintaining a single blog feed.

## Adding Categories to a Post

To assign categories to a blog post, add a `categories` field to the YAML front matter of your post file (located in `_posts/`). 

You can add one or more categories:

```yaml
---
title: "My New Post"
date: 2024-04-14
categories:
  - Research
  - Personal
---
```

## How Categories Work

1. **Single Feed:** All posts, regardless of category, will continue to appear in the main [Blog](/blog/) feed.
2. **Category Archive:** Your site automatically generates a categorized list of posts at [/categories/](/categories/).
3. **URL Structure:** By default, your site includes the category in the URL. A post in the "Research" category will have a URL like `/research/post-title/`.
4. **Multiple Categories:** If a post has multiple categories, the first one listed will typically be used for the primary URL path.

## Best Practices

- **Consistency:** Use consistent capitalization for category names (e.g., always use "Research" instead of mixing "research" and "Research").
- **Excerpts:** Use two newlines to separate your first paragraph; this will be used as the teaser excerpt on the blog feed page.
