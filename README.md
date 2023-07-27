# Magic Replacer for Liquid

This is a straightforward tool made to solve one specific issue: roughly translating some code generated by Doc Factory into HTML with valid Liquid syntax based on a predefined set of rules.

## How to use it

Get the HTML output from Doc Factory, and paste it in the area available in the app, and click the magic wand button on the bottom right corner. You'll get a message indicating your operation was successful. You may get warning messages as well. They appear with a yellow background. Pay attention to those.

Now all you need to do is paste onto the original file, and the content will be updated with the correct replaced values.
## How to run it locally

These are the things you need to run this project in your machine:

- git
- node
- pnpm

Start by cloning the repository. Run these commands in your terminal:

```bash
git clone ssh://git@gitlab.dataguard.de:1023/maffonso/liquid-replacer.git
cd liquid-replacer
pnpm dev
```

You'll see a message in your terminal that looks something like this:

```bash
  VITE v4.4.7  ready in 140 ms

  ➜  Local:   http://127.0.0.1:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help
```

Access the address next to local on your browser, and you should see the app running.