# Hosting CPA/600 on GitHub Pages + embedding in Squarespace

## Why this approach
Pasting the app's code directly into a Squarespace Code Block is what caused your Diopter tool to "work, then break." Squarespace re-sanitizes embedded code on later saves and template changes, strips `<script>` tags, has a character limit your file now exceeds, and reloads pages via AJAX without re-running inline scripts. Hosting the file and pulling it in through an `<iframe>` sidesteps all of that — the iframe loads the real file fresh every time, untouched by Squarespace.

---

## Part 1 — Host the file on GitHub Pages

1. Go to github.com and create a **new repository**. Name it something like `cpa600`. Make it **Public** (Pages needs public on free accounts). Check "Add a README."
2. In the repo, click **Add file → Upload files**.
3. Rename your calculator file to `index.html` before (or after) uploading — naming it `index.html` means the URL is clean with no filename on the end. Drag it in, then **Commit changes**.
4. Go to the repo's **Settings → Pages** (left sidebar).
5. Under "Build and deployment," set **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)**. Click **Save**.
6. Wait ~1–2 minutes. The Pages section will show: *"Your site is live at https://YOURNAME.github.io/cpa600/"*. That URL is your app.

Test it: open that URL in your phone's Safari. The week card should appear and everything should work. (This is also the link you can text the crew directly — it opens in a real browser, no file-viewer problems.)

**Updating later:** when the contract changes or you tweak the app, upload the new `index.html` to the same repo (Add file → Upload → commit, replacing the old one). The live URL updates within a minute. No need to touch Squarespace again.

---

## Part 2 — Embed it in Squarespace

In the Squarespace page, add a **Code Block** (not a Markdown or Embed block) and paste exactly this, replacing `YOURNAME` with your GitHub username:

```html
<iframe
  id="cpa600-frame"
  src="https://YOURNAME.github.io/cpa600/"
  style="width:100%; height:1000px; border:0; overflow:hidden;"
  title="CPA/600 Calculator"
  scrolling="no">
</iframe>
<script>
  window.addEventListener('message', function(e){
    if (e.data && typeof e.data.cpa600Height === 'number'){
      var f = document.getElementById('cpa600-frame');
      if (f) f.style.height = (e.data.cpa600Height + 20) + 'px';
    }
  });
</script>
```

That's it. The `<iframe>` pulls in your hosted app; the little `<script>` listens for the height messages the app sends and resizes the frame to fit, so it grows as weeks are added and never leaves a dead scrollbar.

### If Squarespace strips the `<script>` part
Some Squarespace plans sanitize even Code Blocks. If the auto-resize script gets removed on save (you'll know because the frame stays a fixed height), just delete the `<script>` block and set a generous fixed height instead — change `height:1000px` to something like `height:2200px`. The app still works perfectly; it just won't auto-shrink. Fixed height is the safe fallback.

---

## Part 3 — Quick sanity checklist
- [ ] GitHub repo is **Public**
- [ ] File is named **index.html**
- [ ] Pages shows a live URL and it opens in your phone's Safari
- [ ] Squarespace **Code Block** (not Embed block), iframe `src` has your real username
- [ ] After saving the Squarespace page, **leave and come back** to the page once — this is where the old approach broke; the iframe version should still work

Note: the file has to be named / referenced consistently. If you keep it as `cpa600.html` instead of `index.html`, your URL becomes `https://YOURNAME.github.io/cpa600/cpa600.html` and the iframe `src` must match that exactly.
