npm install tailwindcss
npx tailwindcss init //root

no webpack.js
```js
const tailwindcss = require('tailwindcss');

module.exports = {

  plugins: [
    tailwindcss('./tailwind.config.js'),
    require('autoprefixer'),
  ],
};
```

no app.css

```css
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```

