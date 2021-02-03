Custom Editor for Strapi, It was build on top of CKEditor 5 balloon block editor build
==============================================

[![npm version](https://badge.fury.io/js/%40ckeditor%2Fckeditor5-build-balloon-block.svg)](https://www.npmjs.com/package/@ckeditor/ckeditor5-build-balloon-block)
[![Dependency Status](https://david-dm.org/ckeditor/ckeditor5-build-balloon-block/status.svg)](https://david-dm.org/ckeditor/ckeditor5-build-balloon-block)
[![devDependency Status](https://david-dm.org/ckeditor/ckeditor5-build-balloon-block/dev-status.svg)](https://david-dm.org/ckeditor/ckeditor5-build-balloon-block?type=dev)

The build of CKEditor 5 featuring the balloon and block toolbars. Read more about the [balloon block editor build](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/overview.html#balloon-block-editor) and see the [demo](https://ckeditor.com/docs/ckeditor5/latest/examples/builds/balloon-block-editor.html).

![CKEditor 5 balloon block editor build screenshot](https://c.cksource.com/a/1/img/npm/ckeditor5-build-balloon-block.png)

## Documentation

See:

* [Installation](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/installation.html) for how to install this package and what it contains.
* [Basic API](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/basic-api.html) for how to create an editor and interact with it.
* [Configuration](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/configuration.html) for how to configure the editor.
* [Creating custom builds](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/development/custom-builds.html) for how to customize the build (configure and rebuild the editor bundle).

## Quick start

First, install the build from npm:

```bash
npm install --save @hellospruce/strapi-custom-ckeditor5
```

And use it in your website:

```html
<div id="editor">
	<p>This is the editor content.</p>
</div>
<script src="./node_modules/@ckeditor/ckeditor5-build-balloon-block/build/ckeditor.js"></script>
<script>
	CustomEditor
		.create( document.querySelector( '#editor' ) )
		.then( editor => {
			window.editor = editor;
		} )
		.catch( error => {
			console.error( 'There was a problem initializing the editor.', error );
		} );
</script>
```

Or in your JavaScript application:

```js
import CustomEditor from '@hellospruce/strapi-custom-ckeditor5';