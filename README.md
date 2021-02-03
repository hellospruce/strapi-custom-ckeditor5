Custom Editor for Strapi, It was build on top of CKEditor 5 balloon block editor build

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
or 

```bash
yarn add @hellospruce/strapi-custom-ckeditor5
```

And use it in on strapi:
Create the CKEditor component at:
```bash
strapi/app/extensions/content-manager/admin/src/components/CKEditor/index.js
```
```jsx
import { CKEditor } from '@ckeditor/ckeditor5-react';
import PropTypes from 'prop-types';
import React, { useState, useEffect } from 'react';
import { auth } from 'strapi-helper-plugin'
import cheerio from 'cheerio' 
import CustomEditor from '@hellospruce/strapi-custom-ckeditor5'

const Editor = ({ onChange, name, value }) => {
  const uploadUrl = `${strapi.backendURL}/upload`;
  const [content, setContent] = useState();
  const headers = {
    Authorization: 'Bearer ' + auth.getToken(),
  };
  
  useEffect(()=>{
    if(value){
      const $ = cheerio.load(value, null, false);
      const videos = $('.w-richtext-figure-type-video iframe');
      for(let i=0; i< videos.length; i++){
        let video = videos[i];
        let source = video.attribs['src'];
        video.parent.attribs['data-oembed-url']= source;
      }
  
      setContent($.html());
    }
  }, [value]);

  return (
    <div>
      <CKEditor
        editor={CustomEditor}
        data={content}
        onChange={(event, editor) => {
          const data = editor.getData();
          onChange({ target: { name, value: data } });
        }}
        config={{
          strapiUpload: {
            uploadUrl,
            headers,
          }
        }}
      />
    </div>
  );
};

Editor.propTypes = {
  onChange: PropTypes.func.isRequired,
  name: PropTypes.string.isRequired,
  value: PropTypes.string,
};

export default Editor;
```

Create a component to override the default strapi rich text component:
```bash
strapi/app/extensions/content-manager/admin/src/components/WysiwygWithErrors/index.js
```
```jsx
import React from 'react';
import PropTypes from 'prop-types';
import { isEmpty } from 'lodash';
import { Label, InputDescription, InputErrors } from 'strapi-helper-plugin';
import Editor from '../CKEditor';

const WysiwygWithErrors = ({
  inputDescription,
  errors,
  label,
  name,
  noErrorsDescription,
  onChange,
  value,
}) => {
  let spacer = !isEmpty(inputDescription) ? (
    <div style={{ height: '.4rem' }} />
  ) : (
    <div />
  );
    
  if (!noErrorsDescription && !isEmpty(errors)) {
    spacer = <div />;
  }
  
  return (
    <div
      style={{
        marginBottom: '1.6rem',
        fontSize: '1.3rem',
        fontFamily: 'Lato',
      }}
    >
      <Label htmlFor={name} message={label} style={{ marginBottom: 10 }} />
      <Editor name={name} onChange={onChange} value={value} />
      <InputDescription
        message={inputDescription}
        style={!isEmpty(inputDescription) ? { marginTop: '1.4rem' } : {}}
      />
      <InputErrors
        errors={(!noErrorsDescription && errors) || []}
        name={name}
      />
      {spacer}
    </div>
  );
};

WysiwygWithErrors.defaultProps = {
  errors: [],
  label: '',
  noErrorsDescription: false,
  value: '',
};

WysiwygWithErrors.propTypes = {
  errors: PropTypes.array,
  inputDescription: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.func,
    PropTypes.shape({
      id: PropTypes.string,
      params: PropTypes.object,
    }),
  ]),
  label: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.func,
    PropTypes.shape({
      id: PropTypes.string,
      params: PropTypes.object,
    }),
  ]),
  name: PropTypes.string.isRequired,
  noErrorsDescription: PropTypes.bool,
  onChange: PropTypes.func.isRequired,
  value: PropTypes.string,
};

export default WysiwygWithErrors;
```
