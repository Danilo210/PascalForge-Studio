# PascalForge Studio — IDE visual Pascal no navegador

IDE web inspirada na experiência visual de Delphi e Lazarus. Implementação estática em HTML, CSS e JavaScript, com editor CodeMirror 5 e JSZip incluídos localmente. Pode ser hospedada no Render em **Static Site**, sem Node.js e sem banco de dados.

## Abrir

- Extraia o ZIP.
- Abra `index.html` no Chrome/Edge/Firefox, ou publique os arquivos em um servidor estático.
- O projeto de exemplo aparece na primeira utilização. Os projetos são salvos no `localStorage` do navegador; **exporte ZIP regularmente** para backup e para trabalhar em outro computador.

## O que funciona

1. **Designer visual**: clique/arraste controles da paleta para o formulário, arraste para mover, use a alça inferior direita para redimensionar, clique para selecionar e use Delete/setas.
2. **Paleta de 19 componentes**: TButton, TLabel, TEdit, TMemo, TCheckBox, TRadioButton, TComboBox, TListBox, TPanel, TGroupBox, TImage, TProgressBar, TTrackBar, TSpinEdit, TTimer, TShape, TStringGrid, TDBGrid e TDataSource.
3. **Object Inspector**: edite Caption, Text, coordenadas, tamanhos, cor, listas, Visible, Enabled, Checked e eventos disponíveis.
4. **Editor de código**: syntax highlighting para Object Pascal, numeração de linhas, realce de comentários/strings/palavras reservadas, atalhos, indentação, autocomplete básico via Ctrl+Espaço.
5. **Vários formulários e units**: crie formulários, units Pascal, arquivos e pastas. Abas alternam entre arquivos; units podem referenciar umas às outras usando `uses` no código, como em projetos Pascal convencionais.
6. **Prévia interativa**: abre uma janela de teste para ver e usar controles. Simula atribuições como `Label1.Caption := 'Novo texto'`, `Edit1.Text := 'ABC'` e chamadas simples de `ShowMessage(...)`. **Não interpreta Pascal completo**.
7. **Projetos ZIP**: exportação/importação completa, incluindo metadados `studio-project.json`, `.lpr`, `.lpi`, `.pas`, `.lfm`, imagens e arquivos auxiliares. Unidades Pascal individuais também podem ser importadas como texto editável.
8. **Temas claro e escuro**, modo Design, Código e Dividir, grade opcional, snap a 8px, saída e atalhos.

## Como compilar EXE

A IDE web **não compila Pascal para executável nativo dentro do navegador** e não inclui um compilador em WebAssembly. Isso evita carregar um compilador grande no site, mas exige que você instale o Lazarus/Free Pascal quando precisar compilar:

1. Na IDE, clique **Compilar**.
2. Clique para visitar o site oficial do Lazarus: https://www.lazarus-ide.org/index.php?page=downloads
3. Instale o Lazarus + Free Pascal no Windows. Esse download ocorre **somente por sua ação**, fora da IDE e fora do Render.
4. Na IDE web, clique **Exportar ZIP**.
5. Extraia o ZIP do projeto e abra o arquivo `Projeto1.lpi` (ou o nome escolhido) na IDE Lazarus.
6. Pressione **F9** para compilar e executar, ou rode `Compilar_Windows.bat` se `lazbuild` estiver configurado no `PATH`.

Os fontes exportados são voltados para a **LCL do Lazarus** e sintaxe **Free Pascal**, não para VCL proprietária do Delphi. Não é um port 1:1 do Delphi comercial, e o `.exe` não é gerado na hospedagem estática Render. O projeto exportado não foi submetido a um compilador FPC no ambiente de desenvolvimento; valide o código e as propriedades dos componentes no Lazarus instalado. Alguns recursos (como imagens do TImage) acompanham o ZIP mas precisam ser atribuídos pela propriedade Picture do Lazarus.

### Observações sobre eventos e sincronização

- No painel **Eventos**, clique duas vezes no campo para criar um procedimento no Pascal e abrir o editor.
- O designer mantém campos e eventos em seções delimitadas por comentários `{ STUDIO:... }`. O código de procedimentos já escritos é preservado ao adicionar novos componentes, desde que os marcadores sejam mantidos.
- Não remova esses comentários se quiser continuar sincronizando a interface e o Pascal.
- A simulação do navegador só oferece uma pequena parte da funcionalidade. Componentes de banco de dados precisam ser ligados ao banco no código/Lazarus real; **não existe conexão FireDAC no navegador**.
- O salvamento é local por navegador/perfil e sujeito às cotas de armazenamento. Arquivos de imagem individuais são limitados a 1 MB na importação direta.

## Deploy no Render

1. Envie todos os arquivos e pastas deste projeto, **descompactados**, para a raiz de um repositório do GitHub.
2. Render → **New → Static Site** → selecione o repositório.
3. **Build Command:** `echo "Ready"` (ou deixe vazio se a interface permitir).
4. **Publish Directory:** `.` (um ponto).
5. **Root Directory:** vazio.
6. Publique. O formulário abre em `index.html`.

## Arquivos do projeto

```
PascalForge_Studio/
  index.html
  style.css
  app.js
  pascal-mode.js
  vendor/
    codemirror.js
    codemirror.css
    matchbrackets.js
    comment.js
    show-hint.js
    show-hint.css
    searchcursor.js
    jszip.min.js
    JSZIP_LICENSE.md
  README.md
  LICENCAS.md
  iniciar_windows.bat
```

## Compatibilidade e segurança

- Testado em Chromium (execução de interface e exportação ZIP).
- Código Pascal importado não é executado pelo JavaScript; a prévia só interpreta expressões básicas de eventos conhecidos.
- Projetos e imagens são mantidos no navegador; não são enviados ao Render automaticamente.
- Confirme a licença e a adequação para uso comercial de dependências externas incluídas (MIT; veja `LICENCAS.md`).
