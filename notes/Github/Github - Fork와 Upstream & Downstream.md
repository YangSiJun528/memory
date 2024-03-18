
## 개요 & 요약
github 활동을 하다 보면, `origin/~~`, `local/~~`을 찾아볼 수 있다. 이게 어떤 의미인지 찾아보았다.
- `upstream`: 내가 Fork한 remote repository
- `origin`: 내 소유의 remote repository
- `local`: 내 local repository


## Upstream & Downstream
상대적인 개념으로 흐르는(stream)방향을 나타낸다.     
Upstream -> Downstream 순으로 흐른다.

Github의 내 프로젝트에선, `origin`(remote repository)이 Upstream이고 `local`이 Downstream이다.

다만, 후술할 Fork에서 `upstream`, `origin` 관계도 Upstream & Downstream 관계이다.    
그래서 `origin`, `local`이라고 부르고, Upstream이나  Downstream이라곤 말하지 않는 것 같다.

## Fork
내 소유가 아닌 repository를 내 소유의 repository로 복사하는 걸 말한다.

오픈소스 기여나, 협업 진행 시 Fork를 사용한다.

원래 소유자의 remote repository를 `upstream`, 내가 Fork한 remote를 `origin`이라고 부른다.

# References
- [GitHub에서 협업을 위한 remote repository와 upstream 이해하기](https://pers0n4.io/github-remote-repository-and-upstream)