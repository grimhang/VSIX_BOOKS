---
sort: 3
---

# Visual Studio 기능 확장하기
이 장에서는 Visual Studio의 확장성 모델에 대해 배웁니다. 이를 위해 Visual Studio 사용자 인터페이스와 해당 SDK(소프트웨어 개발 키트)를 살펴보고 사용 가능한 확장성 지점을 확인합니다. 그런 다음 사용자 지정 명령에 대한 기본 확장을 개발하고 코드 창, 도구 메뉴 및 솔루션 탐색기에 통합할 수 있는 방법을 살펴봅니다. 우리는 명령 확장성에 관한 몇 가지 잠재적인 질문에 대해 논의함으로써 이 장을 마무리합니다. 이 장의 끝에서 우리는 Visual Studio 확장성의 기초를 이해하고 우리가 사랑하는 IDE를 위한 실제 확장을 만들기 위한 견고한 토대를 제공하는 기본 확장을 개발할 것입니다.

***
## <font color='dodgerblue' size="6">1) IDE 알아보기 - Visual Studio 유저 인터페이스</font>
확장을 시작하기 전에 Visual Studio의 사용자 인터페이스(UI)를 알아야 합니다. Visual Studio의 UI는 그림 3-1과 같습니다.

![03_01_VisualStudioUi](image/03/03_01_VisualStudioUi.png)   
그림 3-01 Visual Studio 유저 인터페이스

이렇게 하면 Visual Studio의 명명법과 레이아웃을 알 수 있을 뿐만 아니라 확장하려는 확장성 지점과 구성 요소를 알 수 있습니다. 그림 3-1은 이 장을 작성하는 시점의 Visual Studio 2019의 사용자 인터페이스(UI)를 보여줍니다. UI의 여러 섹션에는 번호가 매겨져 있어 쉽게 식별하고 간략하게 논의할 수 있습니다. 그림에서 번호가 매겨진 순서대로 이러한 다양한 구성 요소를 살펴보겠습니다.

1. 메뉴/메뉴바: 비주얼 스튜디오와 자주 사용하는 메뉴 항목으로 구성.  
    a. 파일 - 이 메뉴에는 파일, 프로젝트 및 솔루션의 생성, 열기 및 저장 명령이 포함되어 있습니다.  
    b. 편집 - 코드의 수정, 리팩터링, 잘라내기, 붙여넣기 삭제등의 명령을 포함  
    c. 보기 - Visual Studio IDE의 창을 보기 등 표시하고 볼 수 있는 명령이 포함되어 있습니다.  
    d. 프로젝트 - 프로젝트의 종속성 및 파일을 추가하거나 제거하는 명령이 포함된 메뉴  
    e. 디버그 - 이 메뉴에는 디버깅 도구 및 창을 실행, 디버그 및 활용하는 명령이 포함되어 있습니다.  
    f. 아키텍처 - 도구의 Enterprise 버전에서만 사용할 수 있으며 Community 및 Visual Studio의 다른 버전에서는 사용할 수 없습니다. 이 메뉴에는 솔루션에 대한 코드 맵을 생성하고 UML 탐색기, 계층 탐색기, 클래스 보기, 개체 브라우저 등과 같은 아키텍처와 관련된 다른 창을 표시하는 명령이 포함되어 있습니다.  
    g. 테스트 - 테스트 메뉴 명령은 테스트를 검색, 디버그 및 실행하고 테스트 재생 목록 및 적용 범위 결과를 생성하기 위한 창을 보기 위한 모든 명령을 분류합니다.  
    h. 분석 - 이 메뉴는 코드를 분석하고 코드 측정항목을 계산하는 명령을 그룹화합니다.  
    i. 도구 - Visual Studio IDE의 설정을 사용자 지정하고 변경하는 명령이 포함되어 있습니다.  
    j. 확장 - Visual Studio 2019에 도입된 새로운 메뉴입니다. 확장 및 업데이트 창으로 직접 이동할 수 있습니다. 이전에는 이 창을 보기 위한 탐색이 도구 ➤ 확장 및 업데이트였습니다.  
    k. 창 - 이 메뉴에는 새 창을 추가하는 명령이 포함되어 있습니다. 창을 숨기거나, 보거나, 고정하거나, 탭합니다. 레이아웃을 변경합니다.  
    l. 도움말 - 이 메뉴는 Visual Studio IDE, 제품 등록, 피드백 및 온라인 설명서에 대한 정보를 보기 위한 명령을 그룹화합니다.

2. 툴바  
    도구 모음은 Visual Studio의 최상위 메뉴 바로 아래에 있으며 상황에 맞는 가장 일반적인 명령에 대한 액세스를 제공합니다. 마지막 이미지의 도구 모음에서 볼 수 있는 몇 가지 일반적인 명령은 저장, 모두 저장, 실행 취소 등입니다.

3. 코드 창
    여기에서 코드 파일을 편집하고 코드를 작성합니다. XML, C#, F#, txt, JavaScript, TypeScript, C 또는 설치된 Visual Studio에서 지원하는 기타 파일 형식일 수 있습니다. IntelliSense, 리팩토링, 코드 창의 확장을 통해 전구 스타일 코드 제안을 통해 편집기를 확장할 수 있습니다. 편집기에 의해 노출되는 다른 가능한 확장성 포인트는 여백, 스크롤바, 태그, 장식품, 옵션 등입니다.

4. 코드 창 컨텍스트 메뉴
    이것은 보편적인 명칭이 아닐 수 있습니다. 코드 창을 마우스 오른쪽 버튼으로 클릭하면 코드 파일에서 실행할 수 있는 명령을 나열하는 컨텍스트 메뉴가 표시됩니다. 이 이미지는 코드 창에서 열린 C# 파일의 상황에 맞는 메뉴를 표시합니다. 이 메뉴를 확장하고 새 항목을 추가할 수 있습니다.

5. 에러 리스트
    이름에서 알 수 있듯이 모든 오류, 경고 및 메시지가 이 창에 나열되며 기본적으로(기본 웹 개발 레이아웃에서) Visual Studio 아래쪽에 도킹됩니다. 보기 ➤ 오류 목록을 클릭하거나 Ctrl \ E를 눌러 상단 메뉴 모음에서 열 수 있습니다. 이 창에 표시되는 오류, 경고 및 메시지는 코드를 작성할 때나 IntelliSense에 의해 생성되거나 빌드 또는 코드 분석에 의해 생성됩니다. 오류를 두 번 클릭하면 해당 오류의 파일 및 위치로 이동합니다. 오류, 경고 및 메시지 또는 이들의 조합만 표시하는 필터를 제공합니다. 이 창은 상단에 드롭다운을 제공하여 창에 표시되는 항목의 범위를 문서, 프로젝트 또는 솔루션으로 축소할 수 있습니다. 이 창은 검색을 지원하므로 특정 항목을 더 쉽게 찾을 수 있습니다. 오류 목록의 항목을 표시하고 오류 목록의 항목을 클릭할 때 발생하는 일을 처리하는 확장을 작성할 수 있습니다.

6. 출력 창
    IntelliSense, 솔루션, 소스 제어, 패키지 관리자 등과 같은 Visual Studio IDE의 다양한 구성 요소에서 상태 메시지가 표시되는 곳입니다. 디버깅하는 동안 출력 창은 다음과 같이 디버깅 및 오류 메시지, 경고를 볼 수 있도록 도움이 됩니다. 상태 메시지뿐만 아니라. 출력 창을 활용하는 확장을 작성할 수 있으며 다음 장에서 살펴보겠습니다.

7. 상태 바
    이것은 최신 작업의 상태를 표시하는 Visual Studio IDE의 맨 아래 부분입니다. 마지막 이미지에서 상태가 Ready로 표시됩니다. 확장 기능을 작성하는 동안 상태 표시줄에 상태 메시지를 쉽게 작성할 수 있습니다.

8. 솔루션 탐색기
    Visual Studio를 사용한 적이 있는 모든 개발자는 확실히 솔루션 탐색기에 익숙하고 파일, 프로젝트 및 솔루션을 추가 및 편집하는 데 사용했기 때문에 이 창에 많은 설명이 필요하지 않습니다. 여기에는 우리가 작업 중인 솔루션, 프로젝트 및 파일이 표시됩니다.

9. 솔루션 탐색기 컨텍스트 메뉴
    솔루션 탐색기에서 선택한 항목을 마우스 오른쪽 버튼으로 클릭하면 선택한 항목과 관련된 명령 목록을 표시하는 컨텍스트 메뉴가 나타납니다. 예를 들어 프로젝트가 선택되었을 때 컨텍스트 메뉴를 마우스 오른쪽 버튼으로 클릭하면 Build, Rebuild, Clean, Debug 등과 같이 프로젝트에 적용할 수 있는 항목이 표시됩니다. 이는 파일 및 솔루션에서도 마찬가지로 동일합니다. 이 장의 뒷부분에서 볼 수 있듯이 이 메뉴를 확장할 수 있습니다.

10. 팀 탐색기
    나중에 VSO(Visual Studio Online)로 이름이 바뀐 TFS(Team Foundation Server)와 연결하고 작업하기 위해 나중에 다시 VSTS(Visual Studio Team Services)로 이름이 바뀌었고 현재 Azure DevOps로 알려져 있습니다. 여기에서 GitHub 리포지토리에 연결할 수도 있습니다.

11. 서버 탐색기
    이름에서 알 수 있듯이 시스템에 설치되거나 연결된 서버에 액세스할 수 있는 단일 통합 창을 제공합니다. 가장 일반적인 것은 데이터베이스와 SharePoint 서버입니다. Azure 구독에 연결하면 구독과 연결된 App Service, Classic Cloud Services, SQL Database, Notification Hubs, Virtual Machines 및 Storage와 같은 다양한 클라우드 구성 요소 및 서비스를 볼 수 있습니다.

12. 속성 창
    이 창은 WPF 디자이너 또는 Windows Forms 디자이너 및 편집기와 같은 디자이너에서 선택한 항목의 디자인 시간 속성 및 이벤트를 표시하고 편집(읽기 전용이 아닌 경우)할 수 있는 인터페이스를 제공합니다. 솔루션 탐색기에서 선택한 파일, 프로젝트 및 솔루션 속성을 보는 데에도 사용할 수 있습니다. 속성 창을 표시하려면 디자이너 또는 솔루션 탐색기에서 항목을 선택하고 F4 키를 누릅니다. 이 창은 속성 그리드 컨트롤을 사용하여 속성을 표시합니다. Visual Studio의 사용자 인터페이스를 다시 방문하고 확장성에 대해 간략하게 논의했으므로 이제 Visual Studio의 확장성 모델에 대해 알아볼 차례입니다.

## <font color='dodgerblue' size="6">2) Visual Studio 확장성 모델</font>
이 섹션에서는 Visual Studio 확장성 모델에 대해 설명합니다. 여러분의 마음을 사로잡을 수 있는 분명한 질문은 "확장성 모델을 알아야 하는 이유는 무엇입니까?"입니다. 대답은 간단합니다. WPF/Windows 형식을 모르면 C# 개발자가 Windows용 데스크톱 기반 GUI 응용 프로그램을 만들 수 없는 것처럼 간단합니다. 또는 ASP.NET/ASP.NET Core를 알지 못하는 것과 마찬가지로 이러한 프레임워크를 기반으로 하는 웹 애플리케이션/API를 만들 수 없습니다. 마찬가지로 Visual Studio 확장성 모델을 모르면 좋은 Visual Studio 확장을 만드는 것이 매우 어렵습니다. Visual Studio 확장성 모델을 알게 되면 자신 있게 확장을 만들어 Visual Studio를 확장할 수 있습니다.
마지막 장의 "보일러 플레이트 확장 구조" 섹션에서 Microsoft.VisualStudio.SDK 패키지에 대한 참조가 있는 이 상용구 확장에 대해 논의했음을 상기하십시오. 이 패키지는 메타 패키지이며 Visual Studio를 포함합니다.
소프트웨어 개발 키트(SDK). 이 NuGet 패키지를 독립 실행형 프로젝트에 설치했을 때 150개 이상의 어셈블리가 다운되었습니다(이 장을 작성하는 시점에서 정확히 157개).

```tip
메타 패키지는 함께 의미가 있는 패키지 그룹을 설명하는 특별한 NuGet 패키지입니다. 예를 들어 Microsoft는 .NET Core 앱을 개발하는 데 필요한 모든 NuGet 패키지가 포함된 Microsoft.NETCore.App이라는 메타 패키지를 정의했습니다. 마찬가지로 Microsoft.VisualStudio.SDK는 Visual Studio를 확장하기 위한 확장을 개발하는 데 필요한 여러 NuGet 패키지를 그룹화하는 메타 패키지입니다.
```

이러한 어셈블리는 Visual Studio SDK를 누적적으로 구성합니다. 이러한 157개의 어셈블리 각각에 대해 논의하는 것은 실현 가능하지도 필요하지도 않고 생산적이지도 않으므로 이 섹션에서는 구별되고 가장 중요한 어셈블리만 논의할 것입니다. 또한 이 책의 과정을 통해 확장을 개발하면서 네임스페이스, 인터페이스 및 클래스에 대해 자세히 논의합니다. 우리가 알아야 할 몇 가지 중요한 네임스페이스가 표 3-1에 나열되어 있습니다.

표3-1 Visual Studio SDK에서 중요한 네임스페이스
```
네임스페이스                                    설명
----------------------------------------------  -----------------------------------------------------------------------------------------
Microsoft.VisualStudio.ComponentModelHost       이 네임스페이스는 Visual Studio의 MEF(Managed Extensibility Framework)에 사용되는 
                                                인터페이스 및 GuidList를 정의합니다.
Microsoft.VisualStudio.Shell                    여기에는 Visual Studio 패키지가 정의된 추상 클래스 AsyncPackage 및 Package가 포함됩니다. 
                                                패키지는 Visual Studio IDE를 확장하는 기본 방법입니다.
                                                이 어셈블리는 사용자 도구 창을 만들고 사용자 명령을 정의하는 데에도 사용됩니다.
Microsoft.VisualStudio.Text.Classification      코드 편집기 창에 분류자 및 형식을 추가하는 인터페이스 및 클래스의 컨테이너입니다. 
                                                편집자 분류기라는 이름에서 알 수 있듯이 코드 텍스트를 다른 클래스(예: 키워드, 주석, 
                                                다른 색상 및 테마로 분류하는 데 사용됩니다. 예를 들어 C#의 키워드는 주석과 색상이 다릅니다. 
                                                이는 구문 강조 표시를 제공하는 방법을 제공합니다.
Microsoft.VisualStudio.Text.Editor              여기에는 옵션, 여백, 스크롤바 등과 같이 편집기에서 사용되는 클래스가 포함됩니다.
Microsoft.VisualStudio.Editor                   여기에는 색상, 글꼴 등에 대해 편집기에서 사용하는 인터페이스와 클래스, 편집기 상수가 
                                                포함됩니다.
Microsoft.VisualStudio.Language.Intellisense    이 네임스페이스에는 작업 제안, 전구, 서명 도우미 및 IntelliSense를 담당하는 인터페이스와
                                                클래스가 포함.
Microsoft.VisualStudio.Text                     여기에서 코드 편집기에서 텍스트 선택, 장식, 서식 지정, 개요 및 태그 중괄호 완성 기능을
                                                제공하고 노출하는 유형 및 인터페이스가 있습니다.
Microsoft.VisualStudio.Text.*
Microsoft.VisualStudio.CommandBars              명령 모음, 명령 모음 단추, 이벤트 및 처리기에 대한 정의가 포함되어 있습니다.
Microsoft.VisualStudio.TextTemplating           이 네임스페이스는 텍스트 템플릿 유형, 텍스트 템플릿 엔진 및 텍스트 템플릿 프로세서의
                                                홈입니다. (텍스트 템플릿 변환 도구 키트) 또는 T4 템플릿에 대해 들어본 적이 있다면 동일.
Microsoft.VisualStudio.Threading                이 네임스페이스에는 Visual Studio에서 스레딩을 효과적으로 활용하는 데 도움이 되는 클래스와
                                                형식이 포함되어 있습니다. 여기에는 몇 가지 중요한 유형의 이름을 지정하기 위해 스레딩 도구,
                                                대기 확장, JointableTasks 및 SingleThreadedSynchronizationContext가 포함되어 있습니다.
Microsoft.VisualStudio.ProjectAggregator        여기에는 Visual Studio 프로젝트에 사용되는 두 개의 인터페이스와 하나의 클래스가 포함됩니다.
Microsoft.VisualStudio.ProjectSystem            Visual Studio 프로젝트 시스템 속성을 빌드, 디버그, 참조 및 작업하는 형식은 이 네임스페이스에
                                                정의
Microsoft.VisualStudio.Language                 이 네임스페이스에는 코드 정리, CodeLens 및 IntelliSense에 대한 형식이 있습니다. 
                                                Visual Studio의 코드 편집기에서 사용하는 다른 형식도 포함합니다.
Microsoft.VisualStudio.Utilities                확장 개발에 사용되는 속성은 대부분 이 네임스페이스 아래에 정의됩니다. NameAttribute, 
                                                PriorityAttribute, DisplayAttribute, AppliesToProjectAttribute, OrderAttribute, 
                                                ExportImplementationAttribute, ImportImplementationsAttribute는 이 네임스페이스에 정의된
                                                몇 가지 공통 속성입니다. 또한 편집기에서 사용하는 클래스도 포함합니다.
EnvDTE                                          이 네임스페이스(EnvDTE)는 꽤 오랫동안 Visual Studio에 있었고 수년에 걸쳐 발전해왔기 때문에 
                                                Visual Studio의 다른 버전에 추가된 접미사 80, 90, 90a 및 100을 볼 수 있습니다.
                                                이 네임스페이스에는 Visual Studio의 작업 자동화에 사용되는 인터페이스와 형식이 포함되어 
                                                있습니다. Visual Studio의 이전 버전(VS 2012까지)은 추가 기능을 지원하고 이 네임스페이스를 
                                                광범위하게 사용하는 추가 기능을 만드는 멋진 마법사를 제공했습니다. 최신 VSPackage는 자동화 
                                                모델 활용도 지원합니다.
EnvDTE80
EnvDTE90
EnvDTE90a
EnvDTE100
VSLangProj                                      이 네임스페이스에 정의된 형식과 인터페이스는 언어(C# 또는 VB) 프로젝트 시스템과 자동화에
                                                의해 사용되어 진다.
Microsoft.Build*                                이러한 네임스페이스(Microsoft.Build로 시작하는 네임스페이스)는 MSBuild 인프라에서 사용.
```

이 마지막 표에서는 바다에 떨어지는 것과 같은 소수의 네임스페이스만 보았지만 일반적으로 개발된 Visual Studio 확장에 대해 가장 자주 사용하고 접하게 되는 네임스페이스입니다. 우리는 사랑하는 IDE를 확장하는 이 여정을 진행하면서 몇 가지 네임스페이스에 대해 더 논의할 것입니다.

![03_02_VisualStudioExtModel](image/03/03_02_VisualStudioExtModel.png)   
그림 3-02 Visual Studio 확장성 모델

그림 3-2는 Visual Studio의 고수준 확장성 모델을 보여줍니다. 기본에는 핵심 구성 요소, API 및 COM 인터페이스로 구성된 Visual Studio가 있습니다. 그 위에 Visual Studio를 확장할 수 있는 다양한 방법이 있습니다. 맨 왼쪽에 있는 블록의 이름은 사용자 지정입니다. Visual Studio IDE는 도구 ➤ 사용자 지정을 사용하여 코딩 없이도 확장할 수 있습니다. 이는 명령과 메뉴를 추가/제거하고 위치를 조정할 수 있는 유연성을 제공합니다. 다음 그림은 Visual Studio의 이 사용자 지정 기능을 보여줍니다. 명령을 추가 또는 제거하거나 명령 위치를 변경하여 메뉴 모음, 도구 모음 또는 상황에 맞는 메뉴 명령을 사용자 지정할 수 있습니다. 키보드 버튼을 사용하여 키보드 단축키를 할당하는 옵션도 있습니다. 나는 독자들이 탐색하고 가지고 놀기를 권장합니다.
이 기능은 매우 직관적이기 때문에 자체적으로 사용할 수 있습니다.

![03_03_Customize](image/03/03_03_Customize.png)   
그림 3-03 커스터마이징

Customizations 바로 오른쪽에는 두 가지 더 이상 사용되지 않고 사용되지 않는 확장 방식인 매크로 및 추가 기능이 있습니다. Visual Studio 여정에서 오랜 시간 동안 존재했으며 Visual Studio 2012까지 관련이 있었습니다. 그림 3-3에는 설명 목적으로만 표시되어 있으며 현재 Visual Studio에서 지원하지 않습니다. Visual Studio 자동화 API 즉 EnvDTE를 기반으로 했습니다. 그런 다음 VSPackage를 기반으로 최고 수준의 확장성을 제공하는 Visual Studio SDK가 제공됩니다. 또한 다른 기술 간의 높이 차이를 확인하십시오. Customization은 높이가 가장 작고 Package가 가장 높으며, Package가 가장 높은 확장성을 제공하고 사용자 지정이 가장 낮은 확장성을 제공함을 나타냅니다. VSPackage를 기반으로 모든 확장을 개발할 것입니다. 

중요한 네임스페이스와 SDK 도움말을 아는 것이 도움이 되지만 Visual Studio를 확장하려면 사용할 유형과 인터페이스를 알아야 합니다. 이러한 각 네임스페이스에 대해 정의된 인터페이스와 유형을 보고 나열할 수 있습니다. 그러나 모든 이론과 코드가 없으면 조금 지루할 것입니다. 따라서 이 장의 나머지 부분에서는 확장을 개발하고 중요한 유형과 인터페이스에 대해 논의합니다. Visual Studio에는 확장을 개발하는 동안 클래스에 필요한 네임스페이스를 자동으로 추가하는 확장성 템플릿이 많이 있습니다. 

이 장의 나머지 부분에서 다음 확장을 개발할 것입니다. 

    *  도구 메뉴, 코드 편집기 메뉴, 프로젝트 컨텍스트 메뉴, 코드 창의 사용자 지정 명령. 
    * 키보드 단축키를 명령으로 바인딩. • 아이콘을 명령과 연결합니다. 
    * 명령의 가시성 제어. 
    * 동적 명령. 
    * 사용자 정의 명령으로 이미 존재하는 명령을 연결합니다. 
    * 도구 창 확장. 우리는 과정을 따라 이러한 확장 각각의 설명과 목적에 대해 논의하고 중요한 유형과 인터페이스도 배웁니다.

- ### a. 메뉴와 명령어 확장하기
    이 섹션에서는 도구 메뉴의 명령을 확장하는 Visual Studio 확장을 개발합니다. 그림 3-4는 기본 확장성 명령 템플릿이 도구 메뉴에 명령을 추가하여 보다 직관적임을 보여줍니다.

    ![03_04_MenuAndCommand](image/03/03_04_MenuAndCommand.png)   
    그림 3-03 메뉴와 명령어

    다음으로 솔루션 탐색기의 코드 창 상황에 맞는 메뉴, 프로젝트 상황에 맞는 메뉴, 솔루션 상황에 맞는 메뉴 및 파일 상황에 맞는 메뉴에 명령을 추가하는 방법을 알아보겠습니다. 이 과정에서 Visual Studio 명령 테이블과 .vsct 파일을 올바르게 편집하는 데 도움이 되는 도구에 대해 알아봅니다. 또한 새로 추가된 명령에 키보드 단축키를 바인딩하고 이와 관련된 아이콘을 변경하는 방법도 배웁니다. 마지막으로 이 섹션의 끝 부분에서 이러한 명령의 가시성을 변경하는 방법과 컨텍스트에 따라 명령을 동적으로 추가하거나 제거하는 방법을 봅니다. 코드를 작성해 보겠습니다.

- ### b. 도구 메뉴 확장
    다음 단계에 따라 Visual Studio 도구 메뉴에 대한 확장을 생성하겠습니다. 

    1. 그림 3-5와 같이 VSIX 프로젝트 템플릿을 선택하여 새 Visual Studio 확장성 프로젝트를 생성합니다.
    ![03_05_VsixProjectTemplate](image/03/03_05_VsixProjectTemplate.png)   
    그림 3-05 VSIX 프로젝트 템플릿

    2. 새로 생성된 프로젝트에는 2장에서 논의한 패키지 클래스와 vsixmanifest 파일만 포함되어 있습니다(그림 3-6 참조).    
        ![03_06_FilesInBoilerProject](image/03/03_06_FilesInBoilerProject.png)   
        그림 3-06 보일러플레이트 프로젝트에서의 파일들  

        현재 프로젝트에는 Command 클래스가 없습니다.

    3. 새 명령을 추가하려면 프로젝트를 마우스 오른쪽 버튼으로 클릭합니다. 프로젝트 컨텍스트 메뉴가 표시됩니다. 그런 다음 추가 ➤ 새 항목을 클릭하거나 Ctrl Shift A를 직접 눌러 새 항목을 추가합니다. 그러면 그림 3-7과 같이 새 항목 추가 대화 상자가 열립니다.
        ![03_07_AddNewItem](image/03/03_07_AddNewItem.png)   
        그림 3-04 새 아이템 추가
    
    4. 왼쪽 패널의 Visual C# 항목 확장성 범주에서 VSPackage를 클릭합니다. 다음 네 가지 항목 템플릿이 표시됩니다.  
        a. 비동기 패키지 - Visual Studio에서 로드할 수 있는 비동기 패키지를 만듭니다.  
        b. 명령 - 도구 메뉴에서 맞춤 명령을 만듭니다.  
        c. 비동기 도구 창 - 도구 창을 비동기식으로 로드하는 명령을 사용하여 Visual Studio에서 호스팅할 수 있는 맞춤 도구 창을 만듭니다.  
        d. 도구 창 - 동기식으로 로드하는 명령을 사용하여 Visual Studio에서 호스팅할 수 있는 도구 창을 만듭니다. 새로운 권장 사항은 항상 Async Tool 윈도우를 사용하는 것이므로 이전 버전과의 호환성을 지원하기 위해 제공되는 것 같습니다. 
        
        프로젝트에 새 명령을 추가하기 위해 명령 템플릿을 선택합니다. 파일 이름을 MyCommand.cs로 지정했습니다. 추가 버튼을 클릭합니다.

    5. 프로젝트에 추가된 파일들을 살펴보자. 새로 추가된 파일이 있는 수정된 프로젝트는 그림 3-8과 같으며, 새로 추가된 파일과 참조는 쉽게 식별할 수 있도록 강조 표시됩니다. Windows 양식을 지원하기 위해 참조가 추가되었습니다.  
        
        ![03_08_FilesAddedBy](image/03/03_08_FilesAddedBy.png)   
        그림 3-08 명령어 템플릿에 의해 생성된 파일들

        참조 외에도 세 개의 파일이 더 추가됩니다.  

        a. MyCommand.png  
            리소스 폴더 내에 생성된 생성된 명령어의 이름과 이름이 같은 이미지 스트립 또는 스프라이트(이미지 세트)입니다. 기본 이미지 스프라이트에는 그림 3-9와 같이 6개의 이미지가 포함되어 있습니다. 쉽게 식별할 수 있도록 이미지에 번호를 명시적으로 지정했습니다.
            
        ![03_09_ImageSpriteContainingSixImages](image/03/03_09_ImageSpriteContainingSixImages.png)   
        그림 3-09 6개의 이미지가 포함된 이미지 스프라이트 파일
        
        b. MyCommand.cs  
            새 명령을 추가하기 위해 추가되는 C# 코드 파일이며 명령이 실행될 때 코드를 실행하는 이벤트 핸들러를 포함합니다. 명령 파일을 보면 MyCommand의 인스턴스를 반환하는 Instance라는 공용 정적 속성 외에 세 가지 메서드만 있습니다.

            i. AsyncPackage 및 OleMenuCommandService를 매개 변수로 사용하는 명령의 개인 생성자입니다. 여기에서 명령이 구성되고 명령 서비스에 추가됩니다.
            ii. AsyncPackage에서 OleMenuCommandService를 확인한 후 명령의 개인 생성자를 호출하여 AsyncPackage를 매개 변수로 사용하고 명령의 단일 인스턴스를 초기화하는 공용 정적 InitializeAsync 메서드입니다. 
            iii. 메뉴 항목을 클릭할 때 명령을 실행하는 이벤트 처리기인 Execute라는 개인 콜백 메서드입니다.

        그림 3-10은 이 새로 추가된 명령 클래스를 요약한 것입니다.

        ![03_10_ClasDiagram](image/03/03_10_ClasDiagram.png)   
        그림 3-10 MyCommand.cs 의 클래스 다이어그램

        c. ToolsMenuCommandPackage.vsct  
            파일 확장자 vsct는 Visual Studio 명령 테이블(vsct)을 나타냅니다. 이 파일은 Visual Studio 명령 테이블을 구성하고 VSPackage에 포함된 명령을 설명하는 XML 구성 파일입니다. 또한 명령의 레이아웃과 모양을 제어합니다. 명령은 버튼, 메뉴, 도구 모음 등에 포함될 수 있습니다. 이 구성 파일이 VSCT 컴파일러를 통해 전달되면 Visual Studio에서 이해할 수 있는 바이너리로 변환됩니다. 도구 메뉴에서 명령을 추가하는 코드 스니펫을 확인해보자. 그림 3-11은 vsct 파일의 트리밍된 버전(주석 제거됨)을 보여줍니다. 스니펫에는 vsct 파일의 주요 개념을 논의할 수 있도록 번호가 매겨져 있습니다.
        
        ![03_11_VsctFileCode](image/03/03_11_VsctFileCode.png)   
        그림 3-11 vsct 파일 코드

        vsct 파일의 코드 스니펫에서 다음과 같은 주요 내용을 확인할 수 있습니다.

    6. CommandTable 요소는 vsct 파일의 루트 노드입니다. vsct의 네임스페이스와 스키마는 이 요소의 속성으로 지정됩니다. 이 요소는 명령을 정의하는 모든 요소의 정의를 포함합니다. 명령은 Visual Studio IDE에 표시되는 메뉴 항목, 도구 모음, 단추 또는 콤보 상자일 수 있습니다. 이 명령 테이블에 정의된 것은 무엇이든 포함하는 VSPackage가 Visual Studio IDE에 노출하는 명령의 UI 또는 레이아웃입니다. CommandTable에는 자식으로 Extern, Commands 및 Symbols가 있습니다. CommandTable의 상위 수준 구조는 다음과 같습니다.

    ```xml
    <CommandTable xmlns="http://schemas.microsoft.com/VisualStudio/2005-10-18/CommandTable" 
    xmlns:xs="http://www.w3.org/2001/XMLSchema" >
        <Extern>... </Extern>
        <Include>... </Include>
        <Define>... </Define>
        <Commands>... </Commands>
        <CommandPlacements>... </CommandPlacements>
        <VisibilityConstraints>... </VisibilityConstraints>
        <KeyBindings>... </KeyBindings>
        <UsedCommands... </UsedCommands>
        <Symbols>... </Symbols>
    </CommandTable>
    ```

    다음 섹션에서는 이러한 각 XML 요소의 목적을 요약합니다.

- #### a). Extern
    선택적 요소이며 일반적으로 vsct 컴파일러에 대한 전처리기 지시문을 포함합니다. 이 요소는 컴파일 시 이 .vsct 파일과 병합될 외부 C 헤더(.h) 또는 .vsct 파일을 나타냅니다. href 속성은 파일을 참조하는 데 사용됩니다. .vsct 파일 코드에서 위에서 언급한 파일 이름이 stdidcmd.h임을 알 수 있습니다.  예를 들어:
    ```xml
    <Extern href="stdidcmd.h" />
    ```
- #### b). Include
    이것은 선택적 요소이므로 이전에 표시된 코드 목록에 표시되지 않습니다. 이 요소는 현재 파일에 포함될 수 있는 파일을 지정합니다. href 속성은 파일을 참조하는 데 사용됩니다. 포함 파일에 정의된 모든 기호 및 유형은 컴파일 출력의 일부가 됩니다. 예를 들어:
    ```xml
    <Include href="stdidcmd.h" />
    ```

- #### c). Define
    이것은 다시 선택적 요소입니다. 정의는 이름에서 알 수 있듯이 기호와 해당 값을 정의합니다. 여기에는 두 개의 필수 속성(이름 및 값)과 기호를 평가하는 데 사용할 수 있는 선택적 속성 Condition이 있습니다. 예를 들어:
    ```xml
    <Define name="Mode" value="Standard" />
    ```    

- #### d). Commands
    이것은 또한 선택적 요소입니다. 그러나 이것은 VSPackage에 대한 명령을 정의하는 주요 요소입니다. 여기에는 package라는 속성이 있으며 그 값은 . vsct 파일. 그림 3-11에 표시된 .vsct 코드에서 이는 숫자 4(스니펫에서 세 번 사용됨)로 설명됩니다. .vsct 파일에서 위에서 아래로 이동하면 속성 패키지가 guidToolsMenuCommandPackage로 설정된 Commands 요소에서 #4가 먼저 사용되었음을 알 수 있습니다. 다음 #4는 GuidSymbol 요소가 동일한 이름 guidToolsMenuCommandPackage로 정의되고 해당 값이 GUID(Globally Unique Identifier)인 Symbols 요소 내부에서 볼 수 있습니다. 그림의 오른쪽 하단을 향하여 다시 #4를 볼 수 있습니다. 이것은 Package GUID가 정의된 Package 클래스의 스니펫입니다. (.cs) 파일과 .vsct 파일 뒤에 있는 코드의 GUID 값은 동일합니다. 이 GUID 때문에 명령이 패키지와 연결됩니다. Commands 요소는 Commands의 상위 수준 구조에서 아래와 같이 여러 자식을 가질 수 있습니다.
    
    ```xml
    <Commands package="guidToolsMenuPackage" >
        <Menus>... </Menus>
        <Groups>... </Groups>
        <Buttons>... </Buttons>
        <Combos>... </Combos>
        <Bitmaps>... </Bitmaps>
    </Commands>
    ```    

    Commands의 자식은 5개일 수 있습니다.

- ##### a). Menus
    