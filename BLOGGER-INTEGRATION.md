# Blogger integration

Host the application on GitHub Pages first. Blogger should embed the hosted app; it should not contain Firebase admin credentials or direct database rules.

Example iframe (replace the URL only if your GitHub Pages address changes):

```html
<iframe
  src="https://sonarbanglaadarshoschool.github.io/sbas-school-management/"
  title="SBAS School Management"
  style="width:100%;height:900px;border:0;border-radius:12px;"
  loading="lazy"
  allow="clipboard-read; clipboard-write">
</iframe>
```

Use a Blogger HTML/JavaScript gadget or a page in HTML view. Admin sign-in remains protected by Firebase Authentication and Firestore Rules.
