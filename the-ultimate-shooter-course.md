# Unreal Engine C++ The Ultimate Shooter Course

# Project Setup

...

## Criando e configurando o projeto

Primeiramente vamos criar um projeto, indo em "Games" e depois selecionando "Blank", e selecionando as seguintes configurações:
- C++ selecionado
- Target Platform: Desktop
- Quality Preset: Máximo
- Starter Content: desmarcado
- Raytracing: desmarcado

![](images/2022-10-09-15-35-20.png)

## Organizando os diretorios do projeto

Com o projeto criado, podemos começar criando alguns diretorios para organizar nossos arquivos. Apertando `ctrl + espaço` irá aparecer o "Content Drawer". Clicando no diretorio "Content", com o botão direito vamos criar alguns diretorios:
- "_Game": responsavel por armazenar todos os arquivos relacionados ao nosso jogo. Dentro de games:
  - "Maps"
  - "Character"
  - "GameMode"

## Definindo o Level padrão

Após isso, podemos salvar nosso "Level" atual. Para isso vamos em `File > Save Current Level`, e então selecioanamos o diretorio `_Game/Maps`, nomenado ele de "DefaultMap".

Para garantir que esse "Level" seja carregado toda vez que abrirmos nosso projeto, temos que definir ele como principal. Para isso vamos em `Edit > Project Settings`, e na janela que abriu, na guia lateral "Maps & Modes", podemos selecionar o level que criamos.

![](images/2022-10-09-17-44-22.png)

## Criando o GameMode padrão

Em seguida podemos seguir criando a classe para nosso gamemode que é responsavel por varios recursos globais do jogo como posição inicial do jogador, camera inicial do jogador, etc. Para isso vamos no diretorio `C++ Classes` e com o botão direito na classe "ShooterGameModeBase" selecionamos "Create Blueprint class based on ShooterGameModeBase". Colocamos o nome de "BP_ShooterGameModeBase" e salvamos em `_Game/GameMode`.

![](images/2022-10-09-17-50-08.png)

Com isso será aberto a janela de edição do blueprint com diversas configurações a serem feitas.

![](images/2022-10-09-17-53-11.png)

Para definir esse "GameMode" como padrão, temos que definir ele em:

![](images/2022-10-09-17-55-45.png)

## Criando a classe Character e seu Blueprint

Com nosso projeto criado e configurado podemos seguir para criação da classe do nosso personagem, começando pela criação de uma nova classe C++. Para isso vamos em `C++ Classes/Shooter`, e com o botão direito clicamos em "New C++ Class".

![](images/2022-10-09-17-59-15.png)

E então escolheremos "Character".

![](images/2022-10-09-18-00-34.png)

Vamos colocar o nome de ShooterCharacter e então clicar em "Create Class". Feito isso, nosso editor irá abrir com o conteudo atual de nossa nova classe em dois arquivos `ShooterCharacter.h` e `ShooterCharacter.cpp`.

Depois de criada nossa classe, vamos criar um Blueprint baseado na nossa classe, indo no "Content Drawer" em `C++ Classes/Shooter` e com o botão direito na nosssa classe clicar em "Create Blueprint class based on ShooterCharacter", colocando o nome de BP_ShooterCharacter.

![](images/2022-10-09-19-42-04.png)

Depois de criada, podemos ver que nosso blueprint possui alguns componentes. Componentes são um tipo especial de Objeto que os Atores podem anexar a si mesmos como subobjetos. Os componentes são úteis para compartilhar comportamentos comuns, como a capacidade de exibir uma representação visual, reproduzir sons. Eles também podem representar conceitos específicos do projeto, como a maneira como um veículo interpreta a entrada e altera sua própria velocidade e orientação. Por exemplo, um projeto com carros, aeronaves e barcos controláveis pelo usuário pode implementar as diferenças no controle e movimento do veículo alterando qual Componente um Ator do veículo usa.

![](images/2022-10-09-19-44-23.png)

Temos:
- CapsuleComponent: Geralmente usada para gerenciar a colisão do personagem;
- ArrowComponent: Geralmente usado pra indicar a direção para onde o personagem está olhando;
- Mesh: Usado para guardar o modelo 3D do personagem;
- CharacterComponent: Responsavel por abstrair varios comportamentos comuns de personagens como pular, andar, etc.

## Definindo o Character criado como Pawn padrão

Para definir o character como pawn padrão, temos que ir no blueprint de nosso gamemode, e então esolher em "Default Pawn Class" a classe do nosso character.

![](images/2022-10-09-19-59-58.png)

Depois se dermos "Play", podemos ver o objeto de nosso Character criado.

![](images/2022-10-09-20-01-35.png)

## Exibindo mensagens no LOG do Unreal

Podemos exibir mensagens no OutputLog do Unreal através de da função `UE_LOG(CategoryName, Verbosity, Text)`, onde podemos passar alguns parametros. São eles:

- CategoryName: Onde o log vai ser exibido. Pode ser: `LogTemp`;
- Verbosity: Tipo de log. Pode ser: `Warning` e `Error`;
- Text: Texto a ser exibido. Precisa usar o macro `TEXT("")`;

Para imprimir strings temos que passar o ponteiro da mesma: `UE_LOG(LogTemp, Warning, *someString)`;
Para imprimir inteiros temos que passar o TEXT com formato de inteiro: `UE_LOG(LogTemp, Warning, TEXT("Some Integer: %d"), 2)`;
Para imprimir pontos flutuantes temos que passar o TEXT com formato de ponto flutuante: `UE_LOG(LogTemp, Warning, TEXT("Some float: %f"), 2.5f)`;

...

## Camera Spring Arm

...

## Follow Camera

...

## Controllers and Inputs

...

## Move Forward and Right

...

## Delta Time

...

## Turn at Rate

...

## Mouse Turning and Jumping

...

## Adding Mesh

...

---

# Animations
# Aiming and Crosshairs
# The Weapon
# Item Interpolation
# Reloading
# Advanced Movement
# Ammo Pickups
# Outline and Glow Effects
# Multiple Weapon Types
# Footsteps
# Multiple Characters Meshs
# The Enemy Class
# AI and Behavior Trees
# Khaimera
# Level Creation and Finishing the Game

## Fontes
- https://www.udemy.com/course/unreal-engine-the-ultimate-shooter-course/

