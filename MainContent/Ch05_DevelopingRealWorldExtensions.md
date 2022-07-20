---
sort: 5
---

# 리얼월드 확장 개발
여기에 글

***
## <font color='dodgerblue' size="6">1) 인포바 표시위한 비주얼 스튜디오 확장기능</font>

- ### A. 인포바 확장기능 시작하기

- ### B. 인포바 표시하기
    - **1. 인포바 디스플레이 클래스 작성하기**  
    - **2. 이벤트 핸들러 수정하기**  
- ### C. 확장기능 실행하기

## <font color='dodgerblue' size="6">2) 코드 생성을 위한 비주얼 스튜디오 확장기능</font>

- ### A. 코드 생성 확장기능 시작하기

- ### B. 코드 생성
    - **1. 코드 생성 클래스 만들기**  
    - **2. Command 클래스 업데이트**  

- ### C. 확장기능 실행하기

## <font color='dodgerblue' size="6">3) 요약</font>

## <font color='dodgerblue' size="6">4) Class Reference</font>        
- ### A. Infobar Type System
    - **1. IVsInfoBarHost**  
        정보 표시줄을 배치하는 방법을 알고 있는 호스트 컨트롤을 정의합니다. 이것은 일반적으로 도구 창 또는 정보 표시줄을 호스팅하는 Visual Studio의 기본 창입니다. 다음 메소드를 노출합니다.

        ```
        메쏘드 이름             설명
        ----------------------  -----------------------------------------------------------------------------------------
        AddInfoBar              이 메서드는 호스트가 표시할 정보 표시줄을 추가합니다. 정보 표시줄은 호스트에 추가된
                                순서대로 표시됩니다. 
                                이 메소드는 IVsInfoBarUIElement가 파생되는 매개변수로 IVsUIElement를 사용합니다.
        RemoveInfoBar           정보 표시줄 호스트 컨트롤에서 정보 표시줄을 제거한다. 이 메서드는 IVsUIElement도 매개변수로 사용.
        ```

    - **2. IVsInfoBarUIElement**  
        이름에서 알 수 있듯이 이 유형은 InfoBar의 UI 요소를 정의한다. IVsInfoBarUIElement에 의해 노출되는 메서드는 다음과 같음.

        ```
        메쏘드 이름             설명
        ----------------------  -----------------------------------------------------------------------------------------
        AddInfoBar              이 메서드는 호스트가 표시할 정보 표시줄을 추가합니다. 정보 표시줄은 호스트에 추가된
                                순서대로 표시됩니다. 
                                이 메소드는 IVsInfoBarUIElement가 파생되는 매개변수로 IVsUIElement를 사용합니다.
        RemoveInfoBar           정보 표시줄 호스트 컨트롤에서 정보 표시줄을 제거한다. 이 메서드는 IVsUIElement도 매개변수로 사용.
        ```

    - **3. IVsInfoBarUIFactory**  

    - **4. IVsInfoBar**  

    - **5. InfoBarModel**  

    - **6. IVsInfoBarUIEvents**  

    - **7. IVsInfoBarTextSpan**  

    - **8. IVsInfoBarActionItem**  

    - **9. IVsInfoBarActionItemCollection**  

    - **10. IVsInfoBarTextSpanCollection**  



- ### B. Code Generation Types
    - **1. BaseCodeGenerator**  

        ```
        프로퍼티 이름             설명
        ----------------------  -----------------------------------------------------------------------------------------
        FileNamespace           파일의 네임스페이스를 가져온다.
        InputFilePath           입력 파일의 파일 경로를 가져온다.
        ```

        ```
        메쏘드 이름             설명
        ----------------------  -----------------------------------------------------------------------------------------
        GenerateCode            입력파일에서 코드생성 작업을 수행
        GenerateErrorCallback   이 메서드는 코드 생성 시 오류가 발생하는 경우 쉘 콜백 메커니즘을 통해 오류를 전달합니다.
        GetDefaultExtension     이 메서드는 이 코드 생성기에서 생성된 출력 파일의 기본 확장자를 가져옵니다.
        ```    

    - **2. BaseCodeGeneratorWithSite**  

    - **3. ProvideCodeGeneratorAttribute**  

    - **4. ProvideCodeGeneratorExtensionAttribute**

    - **5. ProvideCodeGeneratorExtensionAttribute**
        