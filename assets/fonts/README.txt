Garet webfont files go here.

assets/css/styles.css declares @font-face for 'Garet' (weights 400 and 700) and
expects these files in this folder:

  Garet-Book.woff2   Garet-Book.woff   Garet-Book.otf   Garet-Book.ttf
  Garet-Bold.woff2   Garet-Bold.woff   Garet-Bold.otf   Garet-Bold.ttf

They were NOT included in the original project files, so all headings are
currently falling back to the system sans-serif. Drop the licensed files in
here and headings will pick up Garet automatically. woff2 alone is enough for
every current browser.
