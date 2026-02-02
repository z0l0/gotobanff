# Twitter/X Open Graph Image Setup Guide

This guide documents how to properly set up Open Graph images for Twitter/X sharing on static sites deployed to Cloudflare Pages.

## The Problem

When sharing a URL on Twitter/X, the preview card doesn't show your custom image, or shows a generic placeholder icon instead.

## Root Causes & Solutions

### 1. Image File Format Issues

**Problem:** Twitter sometimes has issues with RGBA PNG files.

**Solution:** Use JPG format instead.

```bash
# Convert PNG to JPG (macOS)
sips -s format jpeg og.png --out og.jpg

# Or use ImageMagick
convert og.png -quality 85 og.jpg
```

**Optimal specs:**
- Format: JPG (not PNG)
- Dimensions: 1200x630 pixels (Twitter's recommended size)
- File size: Under 5MB (ideally under 300KB)
- Color space: RGB (not RGBA)

### 2. File Path Conflicts with Cloudflare Pages

**Problem:** Cloudflare Pages treats `/og.png` as a route and serves HTML instead of the image.

**Solution:** Put images in a subdirectory like `/images/`.

```bash
mkdir -p images
mv og.png images/og.jpg
```

### 3. Incorrect Content-Type Headers

**Problem:** Cloudflare might serve images with wrong content-type.

**Solution:** Create a `_headers` file in your project root:

```
/images/og.jpg
  Content-Type: image/jpeg
  Cache-Control: public, max-age=31536000

/images/*
  Cache-Control: public, max-age=31536000
```

### 4. Missing or Incorrect Meta Tags

**Problem:** Twitter requires specific meta tags to display cards.

**Solution:** Add ALL of these meta tags to your HTML `<head>`:

```html
<!-- Open Graph meta tags -->
<meta property="og:title" content="Your Site Title">
<meta property="og:description" content="Your site description">
<meta property="og:type" content="website">
<meta property="og:url" content="https://yoursite.com/">
<meta property="og:image" content="https://yoursite.com/images/og.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:image:type" content="image/jpeg">

<!-- Twitter Card meta tags -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Your Site Title">
<meta name="twitter:description" content="Your site description">
<meta name="twitter:image" content="https://yoursite.com/images/og.jpg">
```

**Critical requirements:**
- Use `summary_large_image` for twitter:card (not just `summary`)
- Use absolute URLs (https://yoursite.com/...) not relative paths
- Include both `og:image` AND `twitter:image`
- Include image dimensions (`og:image:width` and `og:image:height`)

### 5. Twitter's Cache

**Problem:** Twitter caches card data aggressively. Even after fixing everything, old cards may still appear.

**Solution:** Force Twitter to refresh by using their Card Validator:

1. Go to https://cards-dev.twitter.com/validator
2. Enter your URL
3. Click "Preview card"
4. This forces Twitter to re-fetch and cache the new data

**Alternative:** Add a cache-busting parameter (though this is temporary):
```html
<meta property="og:image" content="https://yoursite.com/images/og.jpg?v=2">
```

## Complete Checklist

- [ ] Image is JPG format (not PNG)
- [ ] Image is 1200x630 pixels
- [ ] Image is under 300KB
- [ ] Image is in `/images/` subdirectory (not root)
- [ ] `_headers` file exists with correct content-type
- [ ] All meta tags are present in HTML `<head>`
- [ ] Meta tags use absolute URLs (https://...)
- [ ] `twitter:card` is set to `summary_large_image`
- [ ] Both `og:image` and `twitter:image` are present
- [ ] Image dimensions are specified
- [ ] Deployed to production
- [ ] Validated with Twitter Card Validator
- [ ] Tested by posting actual tweet

## Testing & Verification

### 1. Check if image is accessible
```bash
curl -I https://yoursite.com/images/og.jpg
```

Should return:
```
HTTP/2 200
content-type: image/jpeg
```

### 2. Check meta tags as Twitter sees them
```bash
curl -s -A "Twitterbot/1.0" https://yoursite.com/ | grep -E "(og:image|twitter:image|twitter:card)"
```

Should show all your meta tags.

### 3. Use Twitter Card Validator
- URL: https://cards-dev.twitter.com/validator
- Enter your URL and click "Preview card"
- Check the log for errors

### 4. Test in actual tweet composer
- Go to https://twitter.com/compose/tweet
- Paste your URL
- Card should appear with image

## Common Mistakes

1. **Using relative paths** - Always use absolute URLs with https://
2. **Wrong twitter:card value** - Must be `summary_large_image` not `summary`
3. **PNG with transparency** - Use JPG instead
4. **Image too large** - Keep under 300KB
5. **Forgetting _headers file** - Cloudflare needs this for correct content-type
6. **Not clearing Twitter's cache** - Use the validator to force refresh
7. **Image in root directory** - Put in `/images/` to avoid routing conflicts

## File Structure

```
your-project/
├── _headers                    # Cloudflare headers config
├── images/
│   └── og.jpg                 # Your OG image (1200x630)
├── index.html                 # With meta tags
└── other-pages.html           # Each page needs meta tags
```

## Example _headers File

```
/images/og.jpg
  Content-Type: image/jpeg
  Cache-Control: public, max-age=31536000

/images/*
  Cache-Control: public, max-age=31536000
```

## Example HTML Head

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Site Title</title>
    <meta name="description" content="Your site description">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Your Site Title">
    <meta property="og:description" content="Your site description">
    <meta property="og:type" content="website">
    <meta property="og:url" content="https://yoursite.com/">
    <meta property="og:image" content="https://yoursite.com/images/og.jpg">
    <meta property="og:image:width" content="1200">
    <meta property="og:image:height" content="630">
    <meta property="og:image:type" content="image/jpeg">
    
    <!-- Twitter Card -->
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="Your Site Title">
    <meta name="twitter:description" content="Your site description">
    <meta name="twitter:image" content="https://yoursite.com/images/og.jpg">
</head>
<body>
    <!-- Your content -->
</body>
</html>
```

## Troubleshooting

### Image shows but is wrong size
- Check image is exactly 1200x630
- Verify `og:image:width` and `og:image:height` match actual dimensions

### Card shows but no image
- Verify image URL is accessible (curl test)
- Check content-type is `image/jpeg`
- Try converting PNG to JPG
- Clear Twitter cache with validator

### No card at all
- Verify `twitter:card` is set to `summary_large_image`
- Check all required meta tags are present
- Ensure URLs are absolute (https://)
- Test with validator

### Card works in validator but not in tweets
- Wait 5-10 minutes for Twitter's cache to update
- Try posting from different account
- Check if domain is blocked/flagged by Twitter

## Additional Resources

- Twitter Card Validator: https://cards-dev.twitter.com/validator
- Twitter Card Documentation: https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards
- Open Graph Protocol: https://ogp.me/
- Cloudflare Pages Headers: https://developers.cloudflare.com/pages/platform/headers/

## Notes

- Twitter caches card data for 7 days
- Changes may take 5-10 minutes to appear after validation
- Different Twitter clients (web, iOS, Android) may cache differently
- The validator is the most reliable way to force a refresh
