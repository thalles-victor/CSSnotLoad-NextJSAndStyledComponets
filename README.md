# Styled-Components NextJS
Esse script resolve o problema do CSS não carregar, isso vai habilitar o server side render do styled components, basta colar esse script em um arquivo com o nome de _document.js na pasta pages. Aqui etá o link que me ajudou chegar na solução -> 
  
  <a>https://github.com/frontendbr/forum/discussions/2011</a> <p></p>
  <a> https://styled-components.com/docs/advanced#nextjs </a> <p></p>


<p>your-project-path/pages/_document.js</p>

```javascript
import Document from 'next/document'
import { ServerStyleSheet } from 'styled-components'

export default class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const sheet = new ServerStyleSheet()
    const originalRenderPage = ctx.renderPage

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App) => (props) =>
            sheet.collectStyles(<App {...props} />),
        })

      const initialProps = await Document.getInitialProps(ctx)
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>
        ),
      }
    } finally {
      sheet.seal()
    }
  }
}

```
