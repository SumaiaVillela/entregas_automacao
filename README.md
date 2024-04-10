# Trabalhando com automação e construção básica e sites
Este é o repositório de trabalhos desenvolvidos por Sumaia Villela para a disciplina Algoritmos de Automação, do Master em Jornalismo de Dados, Automação e Data Storytelling do Insper.

O trabalho não é feito para ter uma unidade conceitual. São diferentes entregas reunidas, sob orientação do professor, em um mesmo [site](https://entregas-automacao.onrender.com/).

Fazem parte desse "mosaico" de experimentos as seguintes etapas, implementadas via `Flask` e renderizadas no Render:
- Conjunto de páginas estáticas;
- Página dinâmica;
- Recurso de automatização utilizando Google Sheets.

## Páginas estáticas

A primeira etapa foi coletiva. Fizemos a montagem do portfólio de um jornalista (uma pessoa escolhida por grupo) com conhecimentos básicos de `HTML`e `CSS`, contruímos a estrutura de rotas com `Flask` e subimos no Render.

Para subsidiar as decisões estéticas e a organização das informações do portfólio, construímos a história de usuário para o case (destaco somente alguns trechos):

> [!NOTE]
> _Paulo é um profissional multifacetado, com atuação não só no jornalismo tradicional, mas em desenvolvimento web, fotografia, arte e literatura. Ele precisa de um portfólio, em formato de site, que, em ordem de prioridade:_
> 
> - _Reúna seus trabalhos nestes diferentes segmentos;_
> - _Forneça uma biografia para situar possíveis contratantes ou parceiros;_
> - _Ancore os cursos que ele promove._ 
> 
> _(...)_
> 
> _Nesta configuração, o site ficará dividido em três momentos:_
> 
> ***Página 1 (Home/Projetos):*** 
> 
> _Serão listados trabalhos já realizados nas diversas áreas em que atua, demarcando aqueles que são artísticos ou autorais. Uma vez que Paulo trabalha com vários formatos, é preciso adicionar recursos para adição de imagens, vídeos e links que transfiram para sites já desenvolvidos pelo autor._
> 
> ***Página 2 (Cursos):***
> _A página deve listar quais são e seus conceitos, já que os cursos planejados por Paulo estão disponíveis para realização em parceria._  
>  
> ***Página 3 (Biografia):***
> _A biografia deve resumir a carreira, formação e demonstrar habilidades do jornalista, de forma a dar uma visão geral do que os trabalhos mostram de forma > fragmentada. O texto deve ser sucinto e demarcar as áreas de atuação, de modo que seja fácil encontrar as referências que cada perfil de contratante procura. Os contatos também devem ser exibidos, além das redes profissionais do jornalista._


Pudemos explorar estruturas básicas de `HTML` que, mesmo sem ter recursos sofisticados, conseguiram, na nossa avaliação, cumprir o objetivo de atrair e retratar a diversidade de habilidades do jornalista escolhido para a criação do portfólio. Para isso, pesquisamos alguns parâmetros `HTML/CSS` para deixar o site mais fluido. Um exemplo está em botões que levam a páginas desenvolvidas pelo jornalista – ao adicionar o parâmetro `target=_blank`, o site desenvolvido é aberto em uma nova guia. Também construímos um menu com recurso de mudança de cor quando o mouse passa por cima:

```ruby
.menu a:hover {
    color: rgb(164, 43, 43);
}
```

A estrutura do `Flask` e a vinculação ao Render foram etapas individuais. O `Flask` é simples nesta fase. A lógica consiste em criar uma nova rota para cada página e construir uma função para vincular o HTML a ser renderizado naquele endereço:

```python
@app.route("/")
def index():
    return render_template("index.html")
```

## Página dinâmica

**Construí uma ferramenta que ajuda as pessoas a identificarem se os produtos que usam possuem substâncias derivadas do petróleo na sua composição, usando uma base de dados estrturua e Inteligência Artificial (ChatGPT).** O objetivo do projeto é criar um recurso educativo de impacto para conscientizar sobre a onipresença do petróleo em nossas vidas e como falar da redução do uso desse combustível fóssil vai muito além do "combustível".

As informações sobre o projeto implementado na página dinâmica podem ser encontradas no [repositório específico](https://github.com/SumaiaVillela/petroleo_MJD) dessa aplicação.

Compartilho aqui as soluções implementadas no `Flask` para tornar a ferramenta funcional e de uso simplificado pelo usuário.

Na página dinâmica, o `Flask` precisava enviar (`[POST]`) e pegar (`[GET]`) dados do servidor. A parte de envio se relaciona a inputs do usuário em três diferentes formulários: um de imagem, um de texto e outro que é um dropdown com todas as substâncias usadas pelo projeto para fazer a filtragem inicial de substâncias.

Um dos formulários:

```ruby
<div class="input-container">
  <p class="explanation">Envie a imagem de um rótulo ou etiqueta de roupa:</p>
  <p class="guide">A foto ou print precisa ser nítida, de preferência recortada (só os ingredientes). Formatos: JPG, JPEG, PNG</p>
  <form action="/dinamica" method="POST" enctype="multipart/form-data">
    <input type="file" id="imagem" name="image" accept="image/*" required>
    <input type="submit" value="Descobrir">
  </form>
</div>
```

Era preciso fazer a vinculação de variáveis e templates para cada um dos formulários. Havia ainda variáveis de diferentes tipos nos retornos das funções construídas em `Python` para a análise dos inputs, então foi preciso tratar de forma difernete cada condição. Veja o exemplo da estrutura do input de imagem:

```python
    if request.method == 'POST':
        if 'image' in request.files:

            image = request.files['image']
            image_path = "temp_image.jpg"
            image.save(image_path)

            resultado,input = analisa_imagem(image_path)

            if isinstance(resultado, list):
                 return render_template('resultado_lista.html', resultado = resultado, input=input)
            elif isinstance(resultado, str):
                return render_template('resultado.html', resultado = resultado)
```

A estética da ferramenta também foi cuidadosamente pensada para ganhar destaque na página e contar com uma identidade própria. Tentei fazer o melhor com o aprendizado básico de `HTML` e `CSS` que foi adquirido no curso. Para o `CSS`, contei com a assistencia do ChatGPT com mais intensidade, mas sempre no sentido de aprender a estrutura necessária e ir aplicando as especificações que eu queria.

```rust
  /* Estilo dos inputs e botões dentro do box */
  .input-container input[type="file"],
  .input-container input[type="text"],
  .input-container select,
  .input-container input[type="submit"] {
    width: 7cm;
    padding: 10px;
    border: 2px solid #000;
    border-radius: 20px;
    display: inline-block;
    margin-bottom: 10px;
    margin-top: 5px;
    overflow-x: auto;
  }
```

A logo da ferramenta, o infográfico e a arte d ocabeçalho foram feitas por mim no Canva.

## Google Sheets

Uma das etapas da entrega era escolher entre três ferramentas para automatizar uma funcionalidade: Google Sheets, e-mail ou bot do Telegram. O recurso necessário ao projeto foi a planilha eletrônica.

São duas planilhas integradas em diferentes fases do processo. A primeira organiza todas as substâncias derivadas do petróleo que já temos no banco de dados próprio do projeto, para que sejam comparadas com os inputs dos usuários e para a implementação de um dropdown para consulta de todos os termos.

No `Flask, ela é um elemento de `[GET]`:

```python
        # Captura os termos da primeira coluna da planilha
        all_terms = []
        folha = autentica_sheets("1JP0cZ_TVzicML0hQA6mXJA6w8ecqlgZv96RCxpPTucs")
        all_terms.extend(folha.col_values(1)[1:])

        return render_template('dinamica.html', termos=all_terms)
```

Construí uma função para usar essa planilha nas análises, como detalhado no repositório do projeto. Antes foi preciso usar um código para recriar o arquivo json usado para autorizar o acesso à API do Google Sheets (cortesia do professor Álvaro Justen), já que não é possível subir a chave para o GitHub:

```python
arquivo_credenciais = "petroleo-em-tudo-9f1b09147a04.json"
conteudo_credenciais = os.environ["GSPREAD_CREDENTIALS"]
with open(arquivo_credenciais, mode="w") as arquivo:
    arquivo.write(conteudo_credenciais)
conta = ServiceAccountCredentials.from_json_keyfile_name(arquivo_credenciais)
```

A outra planilha é de controle das respostas entregues pelo ChatGPT, quando nenhum termo do input é encontrado na lista de derivados do petróleo. Como a resposta é em parte imprevisível e pode conter imprecisões ou as chamadas "alucinações" da IA, era preciso averiguar, no período de testes, se os prompts estavam retornando as informações desejadas, sem efeitos colaterais significativos. E o controle seguirá sendo feito com o uso de fato da ferramenta.

Para isso, inseri a planilha na parte final da função que analisa os inputs:

```python
    response = requests.post("https://api.openai.com/v1/chat/completions", headers=headers, json=payload)
    resposta = response.json()['choices'][0]['message']['content']

    prova = autentica_sheets("1qqHt7RTxKevh3ZmLRovW_NzWSVNaK3GituqFPVvsCnQ")
    prova.append_row([input, prompt, resposta], value_input_option='RAW')
```

**É importante destacar que ambas as planilhas foram usadas por um especialista para conferir se havia precisão nos retornos. Por isso, a escolha de armazenagem dos bancos de dados foi o Google Sheets, o que permitiu que uma pessoa sem conhecimento em programação e de forma simples e rápida pudesse ter acesso aos dados.**

## Limitações e potenciais

Como explicado de forma mais detalhada no [repositório do projeto](https://github.com/SumaiaVillela/petroleo_MJD), a falta de bases de dados direcionadas para o nosso objetivo que pudessem ser raspadas demandou a criação manual da lista de substâncias, o que limita o universo de investigação. O objetivo, em uma segunda etapa de desenvolvimento da ferramenta, é criar um banco de dados robusto e cientificamente referenciado.

Outra questão foi a armazenagem das imagens enviadas pelo usuário. Não houve tempo hábil para implementação, já que havia um prazo de entrega do trabalho, mas a funcionalidade deve ser adicionada em breve. A intenção é, caso o projeto venha a ter uma escala massiva, poder analisar que tipos de produtos geralmente retornam positivamente para derivados do petróleo, entre outras análises e até a exploração das fotos como recurso visual do projeto.
