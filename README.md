# StyledComponents-NextJS
Para resolver o problema do CSS não aparecer, isso vai habilitar o server side render do styled components, basta colar esse script em um arquivo com o nome de _document.js na pasta pages. Aqui etá o link que me ajudou chegar na solução -> <a>https://github.com/frontendbr/forum/discussions/2011</a>.

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
