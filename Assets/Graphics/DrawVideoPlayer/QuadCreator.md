# 유니티에서 Vertex 직접 찍어서 Quad 그리기

유니티에서 버텍스를 사용하여 직접 Quad를 그려봄으로써,

랜더링의 전반적인 단계들과, 유니티에서는 랜더링 과정을 어떻게 돕고 있는지 알아보았다. 

코드는 여기

[](https://github.com/eunchaeee/unity-snippets/tree/main/Assets/Graphics/DrawVideoPlayer)

아래의 유니티 공식 도큐를 참고하여 작성하였다.

[Unity - Manual:  Example: creating a quad](https://docs.unity3d.com/Manual/Example-CreatingaBillboardPlane.html)

메시 랜더링에 필요한 모든 과정을 직접 해보기 위해, Empty GameObject에 QuadCreator.cs 를 붙이면 Quad가 그려질 수 있도록 작성하였다.

Start 함수 안에 순차적으로 코드를 작성하였다.

다음은 그 설명이다.

### 1. Mesh Filter 붙이기

**Mesh Filter의 역할**

- 메시를 갖고 있음.
- 메시를 화면에 렌더링하기 위해 메시 렌더러에 전달.

### 2. Mesh Renderer 붙이고 Material 부착하기

**Mesh Renderer의 역할**

- 메시 렌더러는 [메시 필터](https://docs.unity3d.com/kr/2021.2/Manual/class-MeshFilter.html)에서 지오메트리를 가져와 게임 오브젝트의 [Transform](https://docs.unity3d.com/kr/2021.2/Manual/class-Transform.html) 컴포넌트에서 정의된 위치에서 렌더링합니다.
- Material 정보를 포함하고 있음.

Material은 Universal Render Pipeline/Lit 쉐이더를 사용해 만들어주었다.

**Material**

머터리얼에 사용하는 셰이더에 따라 머터리얼의 설정 옵션들이 결정된다.

텍스처에 대한 레퍼런스, 타일링 정보, 컬러 틴트 등을 포함한 표면 랜더링 방법이 정의된다.

### 3. Mesh 생성

### 4. Vertices 찍기 (정점 찍기)

**Vertices** 는 그래픽스 공간에서 정점을 뜻한다.

Vector3 배열에 원하는 정점들을 찍어준다.

### 5. Triangle 그리기

mesh.Triangles

메시가 갖고있는 모든 삼각형을 나열한 배열.

따라서 triangles 배열의 크기는 3의 배수이다.

**여기서 버텍스 인덱스는 시계방향으로 적어줘야한다..!**

보통 그래픽스에서 반시계 방향으로 적어야 벡터의 외적을 따라 내가 보는 면이 앞면이 되는데,

유니티에서는 시계 방향으로 정의되면 무조건 앞면이라고 한다.

![[https://docs.unity3d.com/kr/current/Manual/AnatomyofaMesh.html](https://docs.unity3d.com/kr/current/Manual/AnatomyofaMesh.html)](%E1%84%8B%E1%85%B2%E1%84%82%E1%85%B5%E1%84%90%E1%85%B5%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20Vertex%20%E1%84%8C%E1%85%B5%E1%86%A8%E1%84%8C%E1%85%A5%E1%86%B8%20%E1%84%8D%E1%85%B5%E1%86%A8%E1%84%8B%E1%85%A5%E1%84%89%E1%85%A5%20Quad%20%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%84%80%E1%85%B5%20ddb727f0b75248c4891087765286326a/Untitled.png)

[https://docs.unity3d.com/kr/current/Manual/AnatomyofaMesh.html](https://docs.unity3d.com/kr/current/Manual/AnatomyofaMesh.html)

### 6. normal 정보 입력

normal 정보는 버텍스에 수직인 방향을 뜻한다.

노멀 벡터의 방향이 앞면(랜더링 되는 면)이다.

카메라가 보는 방향이 Vector3.forward이므로, 카메라를 마주보도록 Vector3.back으로 설정해 주었다.

normal 벡터 정보는 따로 설정해주지 않아도 괜찮다.

### 7. uv 정보 입력

uv 는 각각의 버텍스가 텍스처의 어느 위치를 랜더링하면 되는지를 나타내는 정보이다.

텍스처 매핑에 사용되며 Vector2로 나타낸다.

![Untitled](%E1%84%8B%E1%85%B2%E1%84%82%E1%85%B5%E1%84%90%E1%85%B5%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20Vertex%20%E1%84%8C%E1%85%B5%E1%86%A8%E1%84%8C%E1%85%A5%E1%86%B8%20%E1%84%8D%E1%85%B5%E1%86%A8%E1%84%8B%E1%85%A5%E1%84%89%E1%85%A5%20Quad%20%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%84%80%E1%85%B5%20ddb727f0b75248c4891087765286326a/Untitled%201.png)

언리얼은 DirectX 방식을, 유니티는 OpenGL 방식을 따라 매핑하면 된다.

### 8. 만든 Mesh를 meshFilter.mesh 에 넣어준다.

### 9. Quad 그리기 성공!

![Untitled](%E1%84%8B%E1%85%B2%E1%84%82%E1%85%B5%E1%84%90%E1%85%B5%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20Vertex%20%E1%84%8C%E1%85%B5%E1%86%A8%E1%84%8C%E1%85%A5%E1%86%B8%20%E1%84%8D%E1%85%B5%E1%86%A8%E1%84%8B%E1%85%A5%E1%84%89%E1%85%A5%20Quad%20%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%84%80%E1%85%B5%20ddb727f0b75248c4891087765286326a/Untitled%202.png)