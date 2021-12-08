# StartLLDB
[LLDB 코스](https://yagom.net/courses/start-lldb/)를 보고 정리를 해놓은 레포지토리입니다.

# LLDB 시작하기
## 팁: LLDB 콘솔은 XCode를 통해 실행된 프로세스만 사용할 수 있는것은 아닙니다. Terminal에서 **lldb -n Finder**를 입력하면 실행중인 앱(Finder)의 프로세스에서도 LLDB를 Attach해서 사용할 수 있습니다. (단, csrutil status를 통해 System Integrity Protection을 Disable시켜야 가능)

## LLDB 명령어 기초 문법
### LLDB 명령어 문법
(lldb) command [subcommand] -option "this is argument"  
 - Command와 Subcommand는 LLDB내 이름입니다.
 - Option의 경우, Command 뒤 어느 곳에든 위치가 가능하며, -로 시작합니다.
 - Argument에 공백이 포함되는 경우도 있기 때문에, ""로 묶어줄 수 있습니다.

> ex) (lldb) breakpoint set --file test.c --line 12  

# Help & Apropos

## Help
해당 문법으로 사용가능한 Subcommand, Option 리스트나 사용법을 보여주는 명령어입니다.  
### #LLDB에서 제공하는 Command가 궁금하다면
```
(lldb) help
```
### 특정 Command의 Subcommand나, Option이 궁금하다면, 
```
(lldb) help breakpoint
(lldb) help breakpoint set
```

## Apropos
원하는 기능의 명령어가 있는지 관련 키워드를 통해 알아볼 수있는 명령어입니다.

### reference count를 체크할 수 있는 명령어가 있을까? 궁금하다면,
```
(lldb) apropos "reference count"
```
결과
```
The following commands may relate to 'reference count':
  refcount -- Inspect the reference count data for a Swift object
```

# Breakpoint
## Breakpoint 만들기
Breakpoint를 만드는 기본적인 명령어 구조는 다음과 같습니다.
```
(lldb) breakpoint set [option] "arguments"
# 줄여서는
(lldb) br s [option] "arguments"
```

### Function Name
특정 이름을 가진 모든 함수에 -name(-n) option을 이용해 break를 걸 수 있습니다.
```
# 함수 이름을 이용해 break
(lldb) breakpoint set --name viewDidLoad
(lldb) b -n viewDidLoad
```
또한, -func-regex (-r) option을 이용해 정규표현식을 활용 할 수 있습니다.
```
(lldb) breakpoint set --func-regex '^hello'
(lldb) br s -r '^hello'
# 'breakopint set --func-regex'는 줄여서 'rb'로 사용 가능
(lldb) rb '^hello'
```

### File
파일의 이름과 line 번호를 이용해 break를 걸기 위해서는, –file (-f), –line (-l) option을 이용할 수 있습니다.
```
# 특정 파일의 20번째 line에서 break 
(lldb) br s --file ViewController.swift --line 20
(lldb) br s -f ViewController.swift -l 20
```
결과
```
Breakpoint 3: where = XCodeGenTest`XCodeGenTest.ViewController.viewDidLoad() -> () + 82 at ViewController.swift:15:15, address = 0x000000010f32d592
```

### Condition
 - condition (-c) option을 이용하면, breakpoint에 조건을 걸 수 있습니다. 

```
# viewWillAppear 호출시, animated가 true인 경우에만 break
(lldb) breakpoint set --name "viewWillAppear" --condition animated
(lldb) br s -n "viewWillAppear" -c animated
```

### Command 실행 & AutoContinue
 - command (-c) option을 이용하면 break시 원하는 lldb command를 실행 할 수 있습니다.

```
(lldb) breakpoint set -n "viewDidLoad" --command "po $arg1" -G1
(lldb) br s -n "viewDidLoad" -C "po $arg1" -G1
```

## Breakpoint 리스트 확인하기
breakpoint list command를 통해 현재 프로그램에 생성되어있는 breakpoint의 목록을 확인할 수 있습니다. 
> hit-count? -> 프로그램 실행 중 활성상태인 breakpoint 지점이 실행되면, Debugger는 hit count를 1씩 늘려가며 기록합니다. 하지만 breakpoint가 걸려있더라도 disable상태이면 count되지 않습니다.  

```
(lldb) breakpoint list
(lldb) br list
# breakpoint 목록 간단하게 출력
(lldb) br list -b
# 특정 id를 가진 breakpoint의 정보만 출력
(lldb) br list 1
```

## Breakpoint 삭제 or 비활성화 시키기
delete, disable Subcommand를 이용해 Breakpoint를 삭제하거나, 비활성화 할 수 있습니다.

```
(lldb) breakpoint delete
(lldb) br de
# 특정 breakpoint 삭제
(lldb) br de 1
# breakpoint 전체 비활성화
(lldb) breakpoint disable
(lldb) br di
# 특정 breakpoint 비활성화
(lldb) br di 1.1
```












