## [React ](#)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="icon" href="favicon.ico" type="image/x-icon" />
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <style>
        .greeting {
            color: #0B57D0;
            text-align: center;
            font-weight: bold;
        }
    </style>
    <title>React DOM</title>
</head>
<body>
    <main></main>
</body>
<script>
    window.onload = function() {
        const element = React.createElement('h1', {className: 'greeting'}, 'Hello, React!');
        const root = ReactDOM.createRoot(document.getElementsByTagName('main')[0]);
        root.render(element);
    }
</script>
</html>
```