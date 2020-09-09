# when using eKoNLPy after installing, I got errors saying tagger() somethingsomething
---

mecab인스톨 완료 후에 eKoNLPy를 사용할 때 tagger()에러가 뜰 때가 었습니다.

execute this on your bash
bash에 다음 커맨드를 입력해주세요
```bash
 <(curl -s https://raw.githubusercontent.com/konlpy/konlpy/master/scripts/mecab.sh)
 $ curl -s https://raw.githubusercontent.com/konlpy/konlpy/master/scripts/mecab.sh
```



잘 동작하는지 확인하기 위해 다음 코드를 실행합니다.
Run following code to find out if our code worked
```bash
from ekonlpy.tag import Mecab
mecab = Mecab()
mecab.pos('금통위는 따라서 물가안정과 병행, 경기상황에 유의하는 금리정책을 펼쳐나가기로 했다고 밝혔다.')
```

다음과 같은 출력이 나오면 성공
following will appear in your python interpreter if the installation went successfully
```bash
> [('금통위', 'NNG'), ('는', 'JX'), ('따라서', 'MAJ'), ('물가', 'NNG'), ('안정', 'NNG'), ('과', 'JC'), ('병행', 'NNG'), (',', 'SC'), ('경기', 'NNG'), ('상황', 'NNG'), ('에', 'JKB'), ('유의', 'NNG'), ('하', 'XSV'), ('는', 'ETM'), ('금리정책', 'NNG'), ('을', 'JKO'), ('펼쳐', 'VV+EC'), ('나가', 'VX'), ('기', 'ETN'), ('로', 'JKB'), ('했', 'VV+EP'), ('다고', 'EC'), ('밝혔', 'VV+EP'), ('다', 'EF'), ('.', 'SF')]
```
