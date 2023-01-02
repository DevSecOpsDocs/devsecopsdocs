<h1 align="center">
  <br>
    <img align="center" src="images/logo.png">
  <br>
  <br>
</h1>

This site is meant to host documentation, as well as blogs, for DevSecOps related topics. This includes things such as securing the software development lifecycle, cloud security, infrastructure as code, and modern cloud native applications. Contributions are welcome!

## Blogs

Contains technical guides that include instructional guidance on solving a given problem. This can also include other off topic items outside of the documentation.

## Local Development

To get started developing on the site, you need to make sure you have `npm` installed and install the dependancies. To update the times on a given page you can perform `date +"%Y-%m-%dT%H:%M:%S%z" | pbcopy`

### Install Dependancies

```bash
npm install
```

### Development Server

```bash
npm run start
```

### Analytics

```
<script defer data-domain="devsecopsdocs.com" src="https://plausible.io/js/script.js"></script>
```


## Credit

- [Hugo](https://gohugo.io/documentation/)
- [Doks](https://getdoks.org/)
- [Doks Child Theme](https://github.com/h-enk/doks-child-theme.git)