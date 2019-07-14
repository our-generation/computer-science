## 릴레이
릴레이는 입력에 따라 내부 스위치를 바꾸어, 출력을 제어하는 장치다.
![릴레이](https://user-images.githubusercontent.com/40727649/61183444-89d36980-a67c-11e9-89a2-939448a46a2d.png)

입력에 전류가 흐르면(1, True) 전자석이 작동한다.  
내부 스위치가 닫혔으므로 출력부분에서 1, True가 검출된다.

## AND 게이트
&& 연산과 같다.  
두개의 입력을 받는다. 둘 모두 true일 때만 출력이 true가 된다.
![and](https://user-images.githubusercontent.com/40727649/61183447-9061e100-a67c-11e9-917e-a323ec1bd780.png)

기호로 표시할 때 다음과 같이 표시한다.  
![and_symbol](https://user-images.githubusercontent.com/40727649/61183449-98ba1c00-a67c-11e9-86dc-dfa51fed8fb1.png)

## OR 게이트
|| 연산과 같다.  
두개의 입력을 받는다. 하나 이상이 true이면 출력이 true가 된다.
![or](https://user-images.githubusercontent.com/40727649/61183446-8c35c380-a67c-11e9-9e09-737b73372ffc.png)

기호로 표시할 때 다음과 같이 표시한다.  
![or_symbol](https://user-images.githubusercontent.com/40727649/61183455-9e176680-a67c-11e9-98a8-db975e9886af.png)

## INVERTOR
! 연산과 같다.  
출력이 true이면 false로, false이면 true로 바꾼다.
![invertor](https://user-images.githubusercontent.com/40727649/61183469-bab39e80-a67c-11e9-8bf9-4fe52b8c04c6.png)

기호로 표시할 때는 끝에 버블을 붙인다.  
![invertor_symbol](https://user-images.githubusercontent.com/40727649/61183470-bab39e80-a67c-11e9-8500-72fb0d8de9fe.png)

## (참고)카르노 맵
불 대수를 표현하기 위한 표.  
![카르노맵](https://user-images.githubusercontent.com/40727649/61183465-ba1b0800-a67c-11e9-8993-bb79d220ba1a.png)

## NAND 게이트
INVERTOR + AND 게이트.  

| NAND  | false | true  |
|-------|-------|-------|
| false | true  | true  |
| true  | true  | false |

- 구현
```cpp
bool nand(bool A, bool B){
    bool answer;
    answer = !(A && B);
    return answer
}
```

## NOR 게이트
INVERTOR + OR 게이트.

| NOR   | false | true  |
|-------|-------|-------|
| false | true  | false |
| true  | false | false |

- 구현
```cpp
bool nor(bool A, bool B){
    bool answer;
    answer = !(A || B);
    return answer;
}
```

## XOR 게이트
입력값 두개를 받아서, 서로 다를 때에만 true를 출력한다.

| NOR   | false | true  |
|-------|-------|-------|
| false | false | true  |
| true  | true  | false |

### 구현방법
- NAND 이용  
![xor-nand](https://user-images.githubusercontent.com/40727649/61183467-ba1b0800-a67c-11e9-8ee3-3ca1e860f9f2.png)

- NOR 이용  
![xor-nor](https://user-images.githubusercontent.com/40727649/61183468-ba1b0800-a67c-11e9-916e-c8aa87b1dc5d.jpg)

- NAND, OR, AND 이용  
![xor-mixed](https://user-images.githubusercontent.com/40727649/61183466-ba1b0800-a67c-11e9-8c68-cbd9db8f3495.png)

- 구현
```cpp
bool xor(bool A, bool B){
    bool answer;
    answer = (!(A && B) && (A || B));
    return answer;
}
```

## 이진 덧셈기

### 반가산기
![반가산기](https://user-images.githubusercontent.com/40727649/61183463-b9827180-a67c-11e9-82cd-a7352454718d.png)  


| 합 | 0 | 1 |
|----|---|---|
| 0  | 0 | 1 |
| 1  | 1 | 0 |



| 자리올림 | 0 | 1 |
|----------|---|---|
| 0        | 0 | 0 |
| 1        | 0 | 1 |

- 구현
```cpp
bool *halfadder(bool A, bool B){
    bool answer[] = {0,0};
    answer[0] = xor(A,B);
    answer[1] = (A && B);
    
    return *answer;
}
```

### 전가산기
![전가산기](https://user-images.githubusercontent.com/40727649/61183464-b9827180-a67c-11e9-8558-1591c0bc1d57.png)

```cpp
bool *fulladder(bool A, bool B, bool carry){
    bool answer[] = {0,0};

    bool S = xor(A, B);
    bool C = A && B;

    answer[0] = xor(A, B);
    answer[1] = (B && carry) || carry;

    return answer;
}
```

## 가산기(바이트)
> 전가산기를 응용해서 바이트 덧셈기를 만들어본다.

```cpp
bool *byteAdder(bool byteA[], bool byteB[]){
    bool *answer = new bool[8];
    bool *carry = new bool[9];
    bool *temp = new bool[2];

    carry[0] = false;
    for (int i = 0; i < 8; i++){
        temp = fulladder(byteA[i], byteB[i], carry[i]);
        carry[i+1] = temp[0];
        answer[i] = temp[1];
    }

    return answer;
}
```
