# Estudo sobre Unreal Engine

# Unreal Engine

## Unreal C++ Classes e metodos
### FMath
`FMath::RandRange(0, 10)` Retorna um número aleatorio que seja 0 ou 10, ou entre 0 e 10.

## Classes e componentes
Nesse exemplo será utilizado **Teste** como nome da classe.
Quando criamos uma classe, temos os seguintes arquivos criados:
> Teste.h
```c++
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "Components/ActorComponent.h"
#include "Teste.generated.h"


UCLASS( ClassGroup=(Custom), meta=(BlueprintSpawnableComponent) )
class BUILDING_ESCAPE_API UTeste : public UActorComponent
{
	GENERATED_BODY()

public:
	UTeste();

protected:
	virtual void BeginPlay() override;

public:
	virtual void TickComponent(float DeltaTime, ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction) override;


};
```
É importante ressaltar que a linha de include da sua propria classe `SuaClasse.generate.h` deve sempre ficar abaixo das inclusões das classes do Unreal.
```c++
...
#include "CoreMinimal.h"
#include "Components/ActorComponent.h"
#include "Teste.generated.h" // Sempre abaixo
...
```

> Teste.cpp
```c++
#include "Teste.h"

UTeste::UTeste()
{
	PrimaryComponentTick.bCanEverTick = true;
}

void UTeste::BeginPlay()
{
	Super::BeginPlay();
}

void UTeste::TickComponent(float DeltaTime, ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction)
{
	Super::TickComponent(DeltaTime, TickType, ThisTickFunction);
}


```
O método `BeginPlay()` é chamado sempre que o jogo começa.

O método `TickComponent()` é chamado a cada quadro do jogo.

No método construtor, a instrução `PrimaryComponentTick.bCanEverTick = true;` indica que o método `TickComponent` sempre será chamado a cada quadro. É possível desativar atribuindo como `false`.

## Acessando o nome de um objeto
```c++
#include "GameFramework/Actor.h" // include necessário

void UYourClass::BeginPlay()
{
  FString ObjectName = GetOwner()->GetName();
}
```

## Acessando localização e transformação do objeto
```c++
#include "GameFramework/Actor.h" // include necessário

void UYourClass::BeginPlay()
{
  FString ObjectPosition = GetOwner()->GetActorLocation().ToString(); // Localização

  FString ObjectTranform = GetOwner()->GetActorTransform().ToString(); // Transformação
  FString AnotherObjectPosition = GetOwner()->GetActorTransform().GetLocation().ToString(); // Localização
}
```

## Organização
Quando se cria um novo projeto, após salvar a cena, crie uma pasta em **Content** chamada **Levels** e mova a cena pra ela.

<div align='center'>
  <img height="500" src="images/8.png">
</div>

<div align='center'>
  <img height="200" src="images/9.png">
</div>

## Importando Meshes Personalizados
Para adicionar meshes a sua cena, no Content Browser, clique em **Import** > **Import To**. Então selecione os arquivos para serem adicionados, e depois de clicar em **Open** irá aparecer o menu de importação do FBX. Se ainda não for trabalhar com as colisões do Mesh, é bom deixar marcado a opção **Generate Missing Collisions**:

<div align='center'>
  <img height="400" src="images/10.png">
</div>

## Criando um material
Para criar um material rapidamente, basta arrastar uma textura para o mesh e então será criado um novo material.

## Corrigindo problemas de textura em um Material
Para abrir o editor de Material, podemos dar dois cliques em um Material, e vamos para a seguinte janela de programação visual do Material:

<div align='center'>
  <img height="400" src="images/11.png">
</div>

Então deletamos a textura associada:

<div align='center'>
  <img height="400" src="images/12.png">
</div>

E adicionamos outra clicando em **Base Color** arrastando para o lado, no qual abrira uma caixa de busca, e por fim digitamos **Texture** e escolhemos **TextureSample**.

<div align='center'>
  <img height="400" src="images/13.png">
</div>

Finalmente selecionamos a textura desejada e aplicamos ao nosso Material.

<div align='center'>
  <img height="400" src="images/14.png">
</div>

Podemos também adicionar novas texturas aos Meshs somente arrastando elas da area **Contents** e puxando para o Mesh.

**É recomendado que se crie novas instancias de um material ao invés de aplicar texturas diretamente ou adicionar ao Mesh, porque assim conseguimos editar unicamente mesh com mesmo material e aplicar diferentes resultados de acordo com cada instancia do material. Para isso clique no material e escolha Create Material Instance**

<div align='center'>
  <img height="400" src="images/15.png">
</div>

## Atribuindo um material padrão ao mesh
Podemos adicionar um material padrão a um mesh e toda vez que criarmos aquele mesh na cena, ele ja vai estar com o material. Para isso deve dar dois cliques no mesh, e então escolher o material desejado:

<div align='center'>
  <img height="400" src="images/16.png">
</div>

## Redimensionando texturas
### Expondo parametros do mesh para o editor
Clicado duas vezes no material, e depois criado um componente através de **UVs**. O nome do componente é **TextureCoordinate**.

<div align='center'>
  <img height="400" src="images/17.png">
</div>

Depois clicamos na area e inserimos outro componente, o **ScalarParameter**. E então o renomeamos para TextureScale, e em seguida definimos seu **Default Value** em 1.0.

<div align='center'>
  <img height="400" src="images/18.png">
</div>

Em seguida criamos outro componente chamado **Multiply**.

<div align='center'>
  <img height="400" src="images/19.png">
</div>

E finalmente realizamos as seguintes conexões: Saída de **Multiply** em **Texture Sample**; Saída de **TexCoord** em **Multiply**; e Saída de **Texture Scale** em **Multiply**.

<div align='center'>
  <img height="400" src="images/20.png">
</div>

Com isso cada instancia do material ganha a seguinte propriedade:

<div align='center'>
  <img height="200" src="images/21.png">
</div>

Através do componente **AppendVector** conseguimos controlar a escala da textura em X e Y:

<div align='center'>
  <img height="300" src="images/22.png">
</div>

## Rotacionando um ator
Podemos rotacionalo através do seguinte método:
```c++
GetOwner()->SetActorRotation(CurrentRotation);
```
Obs.: Ao pegar a rotação de um ator, se ela chegar ou ultrapassar 180°, será retornado um valor negativo. Para contornar isso, pode ser normalizar esse rotação da seguinte forma:
```c++
FRotator CurrentRotation = GetOwner()->GetActorRotation();
if (CurrentRotation.Yaw < 0) {
  CurrentRotation.Yaw = 180.f + FMath::Abs(CurrentRotation.Yaw + 180.f);
}
```

Existem alguns métodos de interpolação que podemos usar para aplicar o efeito de abrir a porta:
```c++
...
float CurrentYaw = FMath::FInterpConstantTo(CurrentRotation.Yaw, 90, DeltaTime, 45.0F); // Interpolação Linear - DEPENDE DO TEMPO E NÃO DA TAXA DE QUADORS
float CurrentYaw = FMath::FInterpTo(CurrentRotation.Yaw, this->TargetCloseYaw, DeltaTime, 2.0F); // Interpolação Exponencial - DEPENDE DO TEMPO E NÃO DA TAXA DE QUADORS
float CurrentYaw = FMath::Lerp(CurrentRotation.Yaw, this->TargetCloseYaw, 0.01f); // DEPENDE DA TAXA DE QUADROS
...
```

Neste exemplo será usado o mesh de uma porta. Primeiro é necessário definir o tipo de mobilidade do objeto como **Movable**:

<div align='center'>
  <img height="300" src="images/23.png">
</div>

Em seguida adicionamos o componente **OpenDoor** a este ator.

>Opendoor.h
```c++
public:
  ...
  void OpenDoor(float DeltaTime);
	void CloseDoor(float DeltaTime);
	FRotator GetActorRotation();
  ...

private:
  ...
  bool DoorOpened = false;
	bool DoorClosed = false;
	bool DoorAction = false;
	float InitialYaw = 0.0f;
	float TargetCloseYaw = 0.0f;
  float TargetOpenYaw = 90.f;
	float CurrentYaw = 0.0;
  ...
};
```

> OpenDoor.cpp
```c++

...
#include "Math/UnrealMathUtility.h"
...

void UOpenDoor::BeginPlay()
{
	...
	Super::BeginPlay();

  FRotator CurrentRotation = GetActorRotationYaw(0);
	this->InitialYaw = CurrentRotation.Yaw;
	this->CurrentYaw = this->InitialYaw;
	this->TargetCloseYaw = this->InitialYaw;
	this->TargetOpenYaw += this->InitialYaw;
	this->DoorOpened = false;
	this->DoorClosed = true;
	this->DoorAction = false;
  ...
}

...
void UOpenDoor::TickComponent(float DeltaTime, ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction)
{
	Super::TickComponent(DeltaTime, TickType, ThisTickFunction);

	if (!this->DoorAction && !this->DoorOpened)
	{
		this->OpenDoor(DeltaTime);
	}
	if (!this->DoorAction && !this->DoorClosed)
	{
		this->CloseDoor(DeltaTime);
	}
}

void UOpenDoor::OpenDoor(float DeltaTime)
{
	FRotator CurrentRotation = GetActorRotationYaw(CurrentYaw);
	CurrentYaw = FMath::FInterpConstantTo(CurrentYaw, TargetOpenYaw, DeltaTime, 45.0F);
	CurrentRotation.Yaw = CurrentYaw;

	if (CurrentRotation.Yaw >= TargetOpenYaw) {
		this->DoorClosed = false;
		this->DoorOpened = true;
		this->DoorAction = false;
	}

	GetOwner()->SetActorRotation(CurrentRotation);
}

void UOpenDoor::CloseDoor(float DeltaTime)
{
	FRotator CurrentRotation = GetActorRotationYaw(CurrentYaw);
	CurrentYaw = FMath::FInterpConstantTo(CurrentYaw, TargetCloseYaw, DeltaTime, 45.0F);
	CurrentRotation.Yaw = CurrentYaw;

	if (CurrentRotation.Yaw <= TargetCloseYaw) {
		this->DoorClosed = true;
		this->DoorOpened = false;
		this->DoorAction = false;
	}

	GetOwner()->SetActorRotation(CurrentRotation);
}

FRotator UOpenDoor::GetActorRotationYaw(float LastYaw)
{
	FRotator CurrentRotation = GetOwner()->GetActorRotation();
	if (CurrentRotation.Yaw < 0) {
		CurrentRotation.Yaw = 180.f + FMath::Abs(CurrentRotation.Yaw + 180.f);
	}
	return CurrentRotation;
}

float UOpenDoor::NormalizeAngle(float Angle)
{
	return (float)((int)Angle % 360);
}
...
```

## Colisão de objetos
Existem algumas formas de trabalharmos com colisão:

Usar colisão complexa, no qual é criado um colisor a partir dos dados reais da geometria. Isso é muito caro em termos de processamento porque conforme suas cenas ficam maiores, podemos esquecer que fizemos isso e acabamos criando mais, deixando talvez o processamento do jogo bem lento.

Usar o BSP e converter o objeto para um mesh estatico e posicionalo onde se deseja trabalhar com a colisão deixando tudo bem simples. Caso o modelo seja relaticamente simples pode se criar a colisão a partir de um modelo primitivo mesmo.

Usar as colisões vindas do proprio artista do mesh.

Para abordar os dois primeiros tipos vamos clicar duas vezes do mesh para ter acesso a ferramenta de edição dele no qual poderemos trabalhar com colisões no mesmo.

<div align='center'>
  <img height="400" src="images/24.png">
</div>

Se clicarmos em **Collision** e depois em **Simple Collision**:

<div align='center'>
  <img height="300" src="images/25.png">
</div>

Poderemos ver uma caixa em verde ao redor do nosso modelo, porém se não tivermos a colisão no nosso modelo vinda de quem fez o modelo, não há nada dentro porque as formas convexas são as mais dificeis de calcular em termos de fisica. Portanto uma colisão simples será assim, colocando uma caixa delimitadora ao redor fazendo com que as partes convexas não possam ser atravessadas.

<div align='center'>
  <img height="300" src="images/26.png">
</div>

Se escolhermos a **Complex Collision** já conseguimos observar que ela consegue delimitar a partes convexas do modelo, e com isso tendo muito mais geometria na colisão podendo assim compromoter o desempenho do jogo.

<div align='center'>
  <img height="300" src="images/27.png">
</div>

Para fins de teste, a melhor maneira seria marcar o tipo de colisão como **Complex Collision**, e depois na esquerda escolher a opção **Use Complex Collision as Simple**.

<div align='center'>
  <img height="400" src="images/28.png">
</div>

## Expondo parametros do código para o editor
Para expor parametros do código e editarmos no editor, colocamos o seguinte marcador acima dos atributos:
```c++
UPROPERTY(EditAnywhere)
float TargetOpenYaw;
```
E depois de compilarmos, conseguimos edita-lo selecionando o componente e então logo abaixo podemos ver nosso atributo.

<div align='center'>
  <img height="400" src="images/29.png">
</div>

## Trigger Volumes
Um Trigger Volume possibilita adicionar ações em determinadas situações. Podemos associar a um listener que espera algo ativar (como passar por uma area) para executar algo.

Podemos acessa-los atraves do painel esquerdo em **Place Actors** > **Basic**.

<div align='center'>
  <img height="400" src="images/31.png">
</div>

Ou simplesmente digitando **Trigger** na caixa de busca

<div align='center'>
  <img height="400" src="images/32.png">
</div>

Podemos criar apenas arrastando para nossa cena. Uma muita muito importante de se fazer é sempre renomear com o nome apropriado, como um ID.

<div align='center'>
  <img height="200" src="images/33.png">
</div>

Para vincular ao nosso codigo e possivelmente expormos como parametro no editor precisamos incluir seu header e depois cria-lo no nosso header.
```c++
...
#include "Engine/TriggerVolume.h"
...

...
UPROPERTY(EditAnywhere)
ATriggerVolume* PressurePlate;
...
```

Finalmente associamos nosso trigger a porta

<div align='center'>
  <img height="400" src="images/34.png">
</div>

### Usando colisão nos volumes
Existem dois tipos:
- Polling no qual é checado toda vez se algo aconteceu.
- Por evento no qual somos notificados quando algo aconteceu.

#### Polling
Para se fazer dessa forma, precisamos escolher o ator que ira acionar o evento e faremos isso expondo o parametro para o editor
```c++
UPROPERTY(EditAnywhere)
ATriggerVolume* PressurePlate;

UPROPERTY(EditAnywhere)
AActor* ActorThatOpen;
```

E então podemos adicionar a seguinte lógica para verificarmos se o ator esta acionando o trigger:
```c++
if (PressurePlate && PressurePlate->IsOverlappingActor(ActorThatOpen))
{
  ...
}
```

Obs.: Durante o teste é necessário escolher o ator que vai acionar o trigger:

<div align='center'>
  <img height="300" src="images/35.png">
</div>
