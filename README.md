# Data Structures & Algorithms in Python

## References

* https://youtu.be/pkYVOmU3MgA?feature=shared
* https://pythondsa.com

## Index

* 0. Strategies to Apply for Solving Problems
* 1. Linear & Binary Search
* 2. Linked Lists
* 3. Complexity Analysis

## 0. Strategies to Apply for Solving Problems

* Step 1. 문제를 명확히 정의하고, input 및 output 형태를 명확히 파악하기

    * 긴 줄글로 되어있는 문제를 간결하고 명확하게 축약하여 정의

* Step 2. 가능한 몇몇 input 및 output의 예시들을 떠올려본다.

    * 특히, 가능한 모든 Edge Cases들을 생각해본다.

* Step 3. 문제에 대한 솔루션을 생각해보기

* Step 4. 생각해본 솔루션 대로 구현해 보고, 버그 발생 시 수정

    * (optional) input 몇 개를 예시로 넣어 테스트

* Step 5. 알고리즘의 복잡도를 계산해 보고, 비효율적인 코드가 있는지 확인해보기

* Step 6. 비효율적인 코드가 있다면 수정하기. 

    * 필요에 따라, Step 3 ~ Step 6의 과정을 반복한다.

## 1. Linear & Binary Search

### Sample Question
    * 7장의 카드들이 준비되어 있다. 이 카드들에는 숫자가 적혀 있으며, 뒤집혀져 보이지 않는 상태이다.
    * 단, 카드들은 큰 숫자부터 작은 숫자 순서대로 놓여 있다.
    * A는 B에게 숫자 하나를 제시하고, 최소 개수의 카드를 해당 숫자를 찾아야 한다.
    * 이 때, 해당 숫자는 몇 번째 카드에 존재하는가?

### Approaches Step By Step

* Step 1.

    * 연속되어 존재하는 숫자가 적혀있는 카드들 => 숫자(자료형)의 리스트(Python의 자료구조)
    * n번째 카드를 뒤집는다 => 리스트의 n번째 인덱스에 접근하여 그 숫자를 조회한다
        * cf) 프로그래밍에서는 현실 세계와는 달리 0번 인덱스부터 시작함을 인지
    * 최소한 적은 카드를 뒤집어야 한다 => 최소한 적은 인덱스 접근을 진행
    => 결론 : 내림차순으로 정렬되어 있는 숫자들의 리스트 속에서 제시받은 숫자의 위치를 찾는 프로그램을 짠다. 가장 적은 횟수로 접근해야 한다.

    * Inputs
        * cards : 내림차순으로 정렬된 리스트 중 아무거나; [13, 11, 10, 7, 4, 3, 1]
        * query : 찾아야 할 숫자; 7
        
    * Outputs
        * 이 경우에는, 결과가 3이 나와야 한다.(7은 리스트의 3번 인덱스에 존재)

    * 이를 바탕으로, 함수의 형태를 대략적으로 생각해볼 수 있다.

        ```
        def locate_card(cards, query): // signature
            pass // no actual codes inside it...
        ```

* Step 2. 

    * point : to cover all the edge cases
    * Test Case를 떠올리는 것이 좋음

        * Test Case는 Dictionary로 관리하면 효율적
        * input과 output 두 가지로 구성

    ```
    test = {
        'input': {
            'cards': [13, 11, 10, 7, 4, 3, 1, 0],
            'query': 7
        },
        'output': 3
    }

    result = locate_card(**test['input'])
    print(result)
    result == locate_card['output'] // true or false
    ```

    * 이 과정을 마쳤어도 아직 코드를 작성하는 것은 지양해야 하며, 문제에서 직면할 수 있는 모든 변수들을 생각하여 리스팅하는 것이 중요하다.

        * 찾고자 하는 숫자가(query)

            * 리스트의 중간에 위치
            * 리스트의 첫 번째에 위치
            * 리스트의 끝에 위치

        * 리스트 숫자들이(cards 리스트)

            * 1개의 숫자로만 이뤄져 있다 => 1개의 숫자 == query
            * 리스트가 찾고자 하는 숫자를 포함하지 않는다 // edge case
            * 리스트가 비어있다 // edge case
            * 리스트가 같은 숫자를 복수로 가지고 있다 
            * query 숫자가 리스트에서 여러 번 등장한다
            * ...

        * edge cases?

            * 흔하지 않거나 극단적인 경우
            * 빈 리스트, 리스트 안에 해당 숫자가 존재하지 않는 경우 등...
            * 문제 풀이 시, 모든 edge case들을 커버할 수 있어야 한다. 예상치 못한 경우가 발생하기 때문

    * 테스트 케이스를 단일 dictionary 대신, 여러 개의 dictionary 형태를 담아놓은 리스트 형태로 관리

    ```
    tests = []
    
    // query occurs in the middle
    tests.append({
        'input': {
            'cards': [13, 11, 10, 7, 4, 3, 1, 0],
            'query': 7
        },
        'output': 3
    })

    // query is the first element
    tests.append({
        'input': {
            'cards': [4, 2, 1, -1],
            'query': 4
        },
        'output': 0
    })

    // query is the last element
    tests.append({
        'input': {
            'cards': [3, -1, -9, -127],
            'query': -127
        },
        'output': 3
    })

    // the list contains one element
    tests.append({
        'input': {
            'cards': [6],
            'query': 6
        },
        'output': 0
    })
    ```

    * Edge Case들을 커버하기 위한 가정들이 주어졌다면 그것들을 잘 읽어본다.

        * 예를 들어, 여기에서는 카드들 중 주어진 숫자가 존재하지 않는 경우 output을 -1로 반환한다고 가정
        * 빈 리스트가 존재해도 -1로 반환한다고 가정
        * 찾고자 하는 숫자가 여러 번 나온다면, 가장 첫 번째로 나오는 인덱스를 반환한다고 가정

    ```
    // does not contain a query
    tests.append({
        'input': {
            'cards': [9, 7, 5, 2, -9],
            'query': 4
        },
        'output': -1 // return -1
    })

    // list is empty
    tests.append({
        'input': {
            'cards': [],
            'query': -7
        },
        'output': -1
    })

    // a number repeats in the list
    tests.append({
        'input': {
            'cards': [8, 8, 6, 6, 6, 6, 6, 3, 2, 2],
            'query': 3
        },
        'output': 7
    })

    // query number occurs multiple times
    tests.append({
        'input': {
            'cards': [8, 8, 6, 6, 6, 6, 6, 3, 2, 2],
            'query': 6
        },
        'output': 2 // 첫 번째로 나오는 인덱스를 반환하는 것이 보통이다.
    })
    ```

    * 주의 : 테스트 케이스를 떠올리는 데에 너무 많은 힘을 쏟지는 말 것... 떠오르는 대로 작성할 것
        * 문제를 구현하는 도중 다시 이 단계로 돌아와서 테스트 케이스를 추가할 수도 있기 때문

* Step 3.

    * 솔루션을 코드로 작성하는 것이 아닌, 자신만의 언어로 설명할 수 있도록 한다.

    * '정확'하면서, '효율적인' 솔루션을 생각해내기
        
    * ex) Brute Force Solution : 일일이 모든 가능한 솔루션을 체크해보는 것(checking all possible answers)

        * 리스트 원소를 하나하나 탐색하는 방법에 대한 예시 => Linear Search Algorithm

            1. position이라는 변수를 생성하여, 0으로 초기화한다. position은 리스트 안의 현재 인덱스를 가리키는 변수이다.
            2. position 변수가 query 변수의 값과 같은지 체크한다.
            3. 만약 같다면, position 변수의 값이 곧 문제의 답이 되며 function으로부터 반환된다.
            4. 만약 다르다면, position을 1씩 늘려가며 같은 과정을 반복한다.
            5. 만약 모든 숫자를 순회하였음에도 정답을 찾지 못하였다면, -1을 반환한다.

        * 이 방법은 '맞는 답'을 찾는 데에는 별 문제가 없으며 매우 간단한 방법

        * Linear Search Algorithm
            * element after element

    ```
    코딩을 시작하기 전에 자신만의 언어로 알고리즘을 표현하는 것이 매우 중요
    상황에 따라 간략히 해도 될 것이며, 자세히 해도 될 것
    "써보는" 것은 명확히 생각하는 데 매우 좋은 도구
    더 명확하게 생각을 '표현'할 수록, 코드로 표현하기 더욱 쉬워질 것
    ```

* Step 4. 

    * Implementation by Linear Search

    ```
    def locate_card(cards, query):
        position = 0

        // loop for repetition
        while True:
            if cards[position] == query:
                // answer was found
                return position

            // increment the position if not found
            position += 1

            // number not found till the end
            if position == len(cards):
                return -1
    ```

    * test the function with the first test case

    ```
    test = {
        'input': {
            'cards': [13, 11, 10, 7, 4, 3, 1, 0],
            'query': 7
        },
        'output': 3
    }

    result = locate_card(**test['input'])

    result == locate_card['output'] // true => 테스트 케이스 충족
    ```

    * 테스트 케이스를 더욱 쉽게 사용할 수 있도록 하는 라이브러리

        * jovian

            * provides a helper function; evaluate_test_case, evaluate_test_cases
            * 함수가 예상된 결과를 산출하는지 알려줄 뿐만 아니라, input, 예상 ouptut, 실제 output, 테스트 결과, 실행 시간까지 보여준다.

        * 사용법

            * evaluate_test_case : 단일 테스트 케이스

            ```
            !pip install jovian --upgrade --quiet
            from jovian.pythondsa import evaluate_test_case
            evaluate_test_case(locate_card, test) // 함수명, '딕셔너리 형태'의 테스트 케이스 순서
            ```

            * evaluate_test_cases : 여러 테스트 케이스들
            ```
            from jovian.pythondsa import evaluate_test_cases
            evaluate_test_cases(locate_card, tests) // tests : 테스트 케이스 딕셔너리의 리스트
            ```

    * <중요> 오류를 디버깅하는 방법
        
        * print 함수를 사용하여 logging하기
        * 함수가 어떻게 동작하고 있는지 visibility를 높일 수 있음
        
        ```
        def locate_card(cards, query):
            position = 0

            // 본격적 연산 진행 전, 값을 확인하기
            print('cards:', cards)
            print('query:', query)

            while True:
                // 현재 접근하고 있는 리스트의 인덱스가 몇 번인지
                print('position:', position)
                if cards[position] == query:
                    return position

                position += 1

                if position == len(cards):
                    return -1            
        ```

    * 오류 발생 : 리스트가 비어있는 경우, 테스트 케이스가 동작하지 못함

        * 이유 : 비어있는 리스트의 0번 인덱스는 접근할 수 없으므로
        * 해결책 : position과 리스트의 크기를 비교하여, position이 리스트의 크기와 같아지는 순간 리스트의 원소에 접근하지 못하도록 조치

        ```
        def locate_card(cards, query):
            position = 0

            // 본격적 연산 진행 전, 값을 확인하기
            print('cards:', cards)
            print('query:', query)

            while position < len(cards):
                // 현재 접근하고 있는 리스트의 인덱스가 몇 번인지
                print('position:', position)
                if cards[position] == query:
                    return position

                position += 1

            return -1 // 루프를 벗어났음은 답을 찾지 못하였다는 의미
        ```

    * <주의> 한 가지 테스트 케이스의 오류를 수정하였으면, 다시 모든 테스트 케이스들이 제대로 동작하는지 테스트하는 것이 필요

    * <추천> 보통의 경우 최적의 솔루션을 생각해 내는 것이 좋겠으나, 최적의 솔루션을 생각해내기 힘든 경우 brute force 방식을 통하여 문제를 푸는 것이 안전할 것

* Step 05.

    * 고안해낸 코드의 복잡도를 계산 => 코드를 최적화 하기 위함
        
    * 위의 문제에서는, 가능한 가장 적은 수의 카드를 뒤집는 방식으로 주어진 숫자를 찾도록 한다. 
        
        * 지금까지는, 카드를 처음부터 끝까지 일일이 한 장씩 뒤집는 방식으로 문제를 해결하는 방법을 찾아냄
            
            * 이것의 복잡도를 측정하는 것은 어떻게 하는 것인가?

            * 문제에서 제시된 사항 : 가장 적은 수의 카드를 뒤집는다 => 가장 적은 수로 리스트의 element에 접근한다

            => 우리가 고안하였던 코드 중에서 리스트의 element에 접근하는 코드를 찾는다
            
            ```
            def locate_card(cards, query):
                position = 0

                while position < len(cards):
                    if cards[position] == query: // element에 접근
                        return position

                    position += 1

                return -1 
            ```

        * 반복문이 한 번 돌아갈 때마다 리스트의 element에 접근함을 알 수 있으며, n개의 element로 이뤄져 있는 리스트의 경우 한 숫자를 찾기 위하여 **"최대"** n번 리스트에 접근해야 함을 알 수 있다.

            * 즉, **최악**의 경우, 숫자를 찾기 위해 n번 접근

        * <최악의 경우> 만약 1분에 한 번만 카드를 뒤집는 것이 허용된다면?

            * 리스트에 30개의 element가 존재한다고 하였을 때, 최악의 경우 숫자를 찾는데 30분의 시간이 소요될 것이다.

            * 그렇다면 이 방법은 이상적인 방법이라고 볼 수 있는가? 5번만 접근해서 답을 찾을 수는 없는 것인가?

    * 결국 Complexity Analysis란, 컴퓨터가 해당 프로그램을 완전히 수행하는데까지 얼마나 많은 시간/공간 등의 컴퓨터 자원이 얼마나 소요되는지 측정하는 것이다.

    * 알고리즘의 Complexity란?
        
        * A measure of the amount of time / space required by an algorithm for an input of a given size "***N***"

        * 별도의 언급이 없는 한, Complexity는 최악의 경우를 상정한다.

            * 따라서, 위에서 언급된 Linear Search의 경우 

                * Time Complexity(=running time of an algorithm)는 N(the number of elements in the list) => The number of operations we perform in each iteration, the time taken to execute a statement

                * Space Complexity(=space needed in the computer's memory/RAM)는 C'. '접근 될' 리스트를 위해서는 'position'이라는 하나의 변수를 담을 공간만을 필요로 하기 때문 


        * Big O Notation
            
            * 최악의 경우에 대한 복잡도는 보통 Big O 표기법을 통해 표현된다.

            * Big O에서는, 알고리즘의 복잡도와 인풋의 사이즈 간의 관계를 표현하기 위해 **상수**와 **낮은 차수**의 변수를 drop한다
                * cN^3 + dN^2 + f => Big O Notation Expression : O(N^3)

            * 따라서, 위의 예시에서 **Linear Search의 시간 복잡도는 O(N)으로, 공간 복잡도는 O(1)로 표현한다.**

* Step 06.

    * Linear Search 방식으로 구현해보고 시간복잡도 및 공간복잡도를 계산해 본 결과, 각각 O(N), O(1)이라는 결론을 얻게 되었다.

    * 상식적으로, 모든 카드가 정렬된 상태에서 리스트의 요소를 하나하나 확인하는 것은 좋지 않은 방법임

    * 더 좋은 방법을 생각해본다.

        * 7이라는 숫자를 찾는 상황에서, 7개의 숫자가 내림차순으로 나열된 리스트가 있다고 가정하고, 만약 랜덤으로 다섯번째(4번 인덱스) 카드를 골랐다고 해본다.

        * 골랐던 카드를 뒤집어 보니, 9가 나옴 => 0번 인덱스부터 해당 카드(4번 인덱스)까지는 답이 될 수 없음을 쉽게 알 수 있음, 이를 통해 7개의 element 중 5개의 element를 한꺼번에 체크한 셈이 된다.

        * 이 원리에 따라, 우리가 뽑는 카드가 원하는 숫자보다 클지 작을지 모르는 상태에서 할 수 있는 최선의 선택은, **가운데 요소를 선택하는 것이 될 수 있다.** => **잘 뽑았든 잘 못 뽑았든 간에 상관 없이** 적어도 한 번의 탐색을 통해 절반은 걸러낼 수 있게 되는 것이다.

        * 계속해서 배제된 범위를 제외하고 정답을 찾을 때까지 중간의 요소를 탐색한다.

        => 이러한 원리의 탐색을 Binary Search라고 부른다.

    * 이제 다시 Step 3 ~ Step 6까지의 과정을 반복한다.

        * Step 2. 솔루션을 자신의 언어로 설명하기
            * 리스트의 중간 element를 선택
            * 찾는 숫자와 같다면, 중간 element를 반환
            * 만약 찾는 숫자보다 접근한 숫자가 적다면, 리스트의 첫 절반 부분만을 다시 탐색
            * 반대의 경우라면, 리스트의 뒤의 절반 부분만을 다시 탐색
            * 끝까지 했음에도 찾지 못했다면, -1 반환

        * Step 3. 솔루션을 구현해보기

            ```
            def locate_card(cards, query):
                lo, hi = 0, len(cards) - 1

                while lo <= hi:
                    mid = (lo + hi) // 2
                    mid_number = cards[mid]

                    print(lo, hi, mid, mid_number)

                    if mid_number == query:
                        return mid
                    elif mid_number < query:
                        hi = mid - 1
                    elif mid_number > query:
                        lo = mid + 1

                return -1
            ```

            * 테스트 해보기

                * [8, 8, 6, 6, 6, 6, 6, 6, 3, 2, 2, 2, 0, 0, 0] 중에서 6은 몇 번째 인덱스에 있는가? 여러 번 나오는 경우 첫 번째 인덱스를 반환해야 함
                * Binary Search로 해결한다면?

            ```
            evaluate_test_cases(locate_card, tests)

            // 인덱스 8번의 테스트를 통과하지 못함 : 찾고자 하는 숫자가 여러 번 리스트에 등장하는 경우

            evaluate_test_case(locate_card, tests[8])
            ```

            * 찾고자 하는 숫자가 여러 번 나오는 경우, 첫 번째로 나오는 인덱스를 찾지 못할 수 있음을 인지하게 됨

                * 따라서, 이러한 예외를 커버하기 위한 조건을 넣어야 할 것

            * 해결 방안은 꽤 간단하게 찾을 수 있음
                
                * 중간 인덱스를 조회했을 때, 찾고자 하는 숫자가 나왔다면 그 전의 숫자와 비교한다. 만약 전의 숫자와 같다면, 그에 대해 조치를 취한다.
                * 조회한 중간 인덱스 숫자를 기준으로 left, found, right 개념을 도입
                * Special case를 커버하고자 할 때, helper function 도입 : test_location 도입
                    * Helper Function의 Body는 7줄 ~ 8줄이 적절하다. 한 번에 캐치할 수 있는 정보가 그 정도일 것이기 때문


            ```
            def test_location(cards, query, mid):
                mid_number = cards[mid]
                print(mid, mid_number)

                if mid_number == query:
                    if mid - 1 >= 0 and cards[mid - 1] == query:
                        return 'left'
                    else:
                        return 'found'

                elif mid_number < query:
                    return 'left'
                else:
                    return 'right'


            def locate_card(cards, query):
                lo, hi = 0, len(cards) - 1

                while lo < hi:
                    print(lo, hi)
                    mid = (lo + hi) // 2
                    result = test_location(cards, query, mid)

                    if result == 'found':
                        return mid
                    elif result == 'left':
                        hi = mid - 1
                    else:
                        lo = mid + 1

                return -1
            ```

        * 해당 테스트 케이스와 모든 테스트 케이스들을 다시 테스트

    * Step 5.

        * 시간 복잡도

            * 알고리즘의 반복 횟수를 세보기
                * 초기 길이 : N
                * 첫 번째 시행 후 길이 : N / 2
                * 두 번째 시행 후 길이 : N / 4 = N / (2 ^ 2)
                * 세 번째 시행 후 길이 : N / 8 = N / (2 ^ 3)
                * ... 각 시행 시마다 리스트의 사이즈는 절반씩 줄어듦
                * k 번째 시행 후 길이 : N / 2^k
                    * k번째 시행이 마지막 iteration이라고 가정하면, 남은 원소 수는 1이 된다
                    * 따라서, N / 2^k = 1 ; k = log N

            * O(log N)

        * 공간 복잡도
            
            * O(1)

### Binary Search vs Linear Search

* 앞의 예제를 통하여, 아래와 같이 두 알고리즘 상에는 시간 복잡도 상 차이가 있음을 알 수 있었다.
    * Binary Search : O(log N)
    * Linear Search : O(N)

* 그 차이를 비교하기 위하여, 극단적으로 큰 크기의 테스트 케이스를 이용해 본다.
    
    ```
    def locate_card_linear(cards, query):
        position = 0

        // 본격적 연산 진행 전, 값을 확인하기
        print('cards:', cards)
        print('query:', query)

        while position < len(cards):
            // 현재 접근하고 있는 리스트의 인덱스가 몇 번인지
            print('position:', position)
            if cards[position] == query:
                return position

            position += 1

        return -1 // 루프를 벗어났음은 답을 찾지 못하였다는 의미


    large_test = {
        'input': {
            'cards': list(range(10000000, 0, -1)),
            'query': 2 // approximately the worst case for both of Binary & Linear Search
        },
        'output': 9999998
    }

    result, passed, runtime = evaluate_test_case(locate_card_linear, large_test, display = False)
    print(f'Result : {result}\nPassed: {passed}\nRuntime: {runtime}')

    // compare
    result, passed, runtime = evaluate_test_case(locate_card, large_test, display = False)
    print(f'Result : {result}\nPassed: {passed}\nRuntime: {runtime}')
    ```

* Binary Search<O(log N)>가 Linear Search<O(N)>보다 약 55,000배 빠름을 알 수 있음

https://github.com/kmkim2689/compose-mvvm-cryptocurrency/assets/101035437/0a444832-8636-4b19-b9f0-ad840658e4d6

    * 보통 코딩 테스트에서는, Brute Force 방식으로 O(N^2)으로 구현 가능한 것을 O(N) 내지 O(log N)으로 구현할 것이 요구된다.

### Generic Binary Search

* Binary Search를 활용하여, 많은 문제를 풀 수 있다.
* Binary Search를 사용하는 문제들에서 일반적/공통적으로 행해야 하는 것들은 다음과 같다.

1. 현재/주어진 포지션을 기준으로 정답이 앞에 있는지, 뒤에 있는지 찾음
    * 특정 범위 내에서 정답의 포지션을 찾는 문제
2. 리스트의 중간 지점(포지션 인덱스) 및 중간 지점의 원소를 가져옴
3. 만약 해당 원소가 정답이라면, 종간 지점 인덱스를 반환
4. 만약 정답이 앞에 있다면, 앞의 절반 부분을 다시 탐색
5. 만약 정답이 뒤에 있다면, 뒤의 절반 부분을 다시 탐색

* Binary Search 기능들 전용 함수를 구현해놓고 시작하는 것이 좋다.
```
def binary_search(lo, hi, condition):
    while lo <= hi:
        mid = (lo + hi) // 2
        result = condition(mid)
        if result == 'found':
            return mid
        elif result == 'left':
            hi = mid - 1
        else:
            lo = mid + 1

    return -1
```

* 문제에 따라 binary_search 함수를 이용하는 함수를 구현한다.

    * 예를 들어, 카드의 위치를 찾는 문제라면

        ```
        def locate_card(cards, query):

            def condition(mid):
                if cards[mid] == query:
                    if mid > 0 and cards[mid - 1] == query:
                        return 'left'
                    else:
                        return 'found'
                elif cards[mid] < query:
                    return 'left'
                else:
                    return 'right'

            return binary_search(0, len(cards) - 1, condition)
        ```

    * 다른 예시로, 특정 숫자가 시작되는 위치와 끝나는 위치를 찾는 문제라면

        ```
        def first_position(nums, target):
            def condition(mid):
                if nums[mid] == target:
                    if mid > 0 and nums[mid - 1] == target:
                        return 'left'
                    return 'found'
                elif nums[mid] < target:
                    return 'right'
                else:
                    return 'left'
            
            return binary_search(0, len(nums)-1, condition)

        def last_position(nums, target):
            def condition(mid):
                if nums[mid] == target:
                    if mid < len(nums)-1 and nums[mid + 1] == target:
                        return 'right'
                    return 'found'
                elif nums[mid] < target:
                    return 'right'
                else:
                    return 'left'
            
            return binary_search(0, len(nums)-1, condition)
        
        def first_and_last_position(nums, target):
            return first_position(nums, target), last_position(nums, target)
        ```