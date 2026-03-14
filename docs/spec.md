# Simple Wireframe Builder (Single File) – Especificação

## Objetivo

Criar uma aplicação web **100% client-side** para desenhar **wireframes simples de telas e formulários**, com aparência semelhante ao **Bootstrap 5**, permitindo exportar os protótipos como **PNG**.

A aplicação deve funcionar com **apenas um arquivo `index.html`**, contendo **HTML, CSS e JavaScript embutidos**.

O objetivo principal é permitir criar **prototipações rápidas de interfaces** que possam ser exportadas como imagem para orientar **outras IAs a gerar sistemas completos**.

---

# Requisitos Técnicos

## Restrições

A aplicação deve:

* Ser composta por **um único arquivo**:

```
index.html
```

* Não usar backend
* Não usar build tools
* Não depender de frameworks pesados
* Funcionar **offline**
* Utilizar **LocalStorage** para persistência

Bibliotecas permitidas (via CDN):

* Bootstrap 5 (CSS apenas)
* html2canvas (para exportar PNG)

---

# Estrutura Geral da Aplicação

A interface deve ter três áreas principais:

```
+-------------------------------------------------------+
| Toolbar Superior                                      |
+-------------+-----------------------------------------+
| Sidebar     | Canvas de Wireframe                     |
| Componentes |                                         |
|             |                                         |
|             |                                         |
+-------------+-----------------------------------------+
```

## 1. Toolbar

Funções:

* Novo wireframe
* Salvar
* Carregar
* Exportar PNG
* Mostrar/ocultar grid
* Zoom (opcional)

Botões sugeridos:

```
New
Save
Load
Export PNG
Toggle Grid
Clear
```

---

## 2. Sidebar de Componentes

Lista de elementos arrastáveis para o canvas.

Componentes iniciais:

### Estruturais

* Container
* Row
* Card
* Datatable
* Navbar

### Formulário

* Label
* Input
* Textarea
* Select
* Checkbox
* Radio
* Button

### Conteúdo

* Heading
* Paragraph
* Image Placeholder
* Divider

---

# Canvas de Wireframe

O canvas é a área onde o usuário desenha a tela.

Deve ser implementado usando:

```
<div id="canvas"></div>
```

Cada elemento inserido deve ser um `div` posicionado absolutamente.

Exemplo:

```
<div class="wireframe-element" style="top:100px;left:200px;width:300px;height:80px">
```

---

# Sistema de Grid

O canvas deve ter **grid visível**.

### Configuração padrão

```
gridSize = 10px
```

### Renderização

Pode ser feito via CSS:

```
background-size: 10px 10px;
background-image:
linear-gradient(to right, #eee 1px, transparent 1px),
linear-gradient(to bottom, #eee 1px, transparent 1px);
```

---

# Snap to Grid

Todos os elementos devem aderir automaticamente à grid.

Função de snap:

```javascript
function snap(value, gridSize){
  return Math.round(value / gridSize) * gridSize;
}
```

Aplicar snap em:

* posição (top, left)
* largura
* altura

---

# Interações do Usuário

## Arrastar Componentes

Usuário arrasta da sidebar para o canvas.

Fluxo:

```
dragstart
dragover
drop
```

Ao soltar:

1. criar elemento
2. posicionar com snap
3. aplicar estilo wireframe

---

## Selecionar Elemento

Clique no elemento.

Mostrar borda:

```
outline: 2px dashed #007bff;
```

---

## Mover Elemento

Elementos devem poder ser arrastados dentro do canvas.

Ao mover:

* aplicar snap to grid

---

## Redimensionar

Elementos devem possuir **handles de resize**.

Posições:

```
bottom-right
bottom-left
top-right
top-left
```

---

# Estilo Wireframe

Todos os componentes devem parecer **esboços**.

### Características

* fundo branco
* borda cinza
* texto cinza
* aparência de protótipo

### CSS Base

```css
.wireframe-element{
  border:1px solid #999;
  background:#fff;
  font-size:14px;
  color:#555;
  box-sizing:border-box;
}
```

---

# Aparência Inspirada no Bootstrap

Os componentes devem **simular Bootstrap**.

Exemplos:

### Button

```
[ Button ]
```

### Input

```
+----------------------+
| input placeholder    |
+----------------------+
```

### Card

```
+----------------------+
| Card Title           |
|                      |
| Card content         |
+----------------------+
```

---

# Modelo de Dados

Os elementos devem ser armazenados em um array:

```javascript
{
  id: "el_123",
  type: "input",
  x: 200,
  y: 120,
  width: 300,
  height: 40,
  text: "Email"
}
```

Lista completa:

```
elements = []
```

---

# Persistência (LocalStorage)

Salvar usando:

```
localStorage
```

Chave:

```
wireframe-project
```

Salvar:

```javascript
localStorage.setItem("wireframe-project", JSON.stringify(elements))
```

Carregar:

```javascript
elements = JSON.parse(localStorage.getItem("wireframe-project"))
```

Ao carregar:

* reconstruir todos os elementos no canvas.

---

# Exportar PNG

Utilizar:

```
html2canvas
```

Fluxo:

```javascript
html2canvas(canvas).then(canvas=>{
   const link = document.createElement('a')
   link.download = "wireframe.png"
   link.href = canvas.toDataURL()
   link.click()
})
```

Antes da captura:

* esconder grid
* remover seleção

Após captura:

* restaurar UI

---

# Organização do Código

Mesmo sendo um único arquivo, separar logicamente:

```
index.html
```

Estrutura:

```
<html>

<head>
CSS
</head>

<body>

HTML UI

<script>

STATE
GRID
COMPONENT REGISTRY
DRAG/DROP
MOVE/RESIZE
RENDER
SAVE/LOAD
EXPORT

</script>

</body>
</html>
```

---

# Sistema de Componentes

Criar um registry:

```javascript
const components = {
  button:{},
  input:{},
  textarea:{},
  card:{},
  label:{}
}
```

Cada componente define:

```
defaultWidth
defaultHeight
render()
```

---

# Performance

Mesmo com muitos elementos:

* evitar re-render completo
* manipular apenas elementos alterados

---

# Critérios de Aceitação

A aplicação estará correta se:

* rodar abrindo apenas `index.html`
* permitir arrastar componentes
* possuir grid visível
* possuir snap to grid
* permitir mover e redimensionar
* salvar no localStorage
* carregar wireframes salvos
* exportar PNG
* ter aparência de wireframe
