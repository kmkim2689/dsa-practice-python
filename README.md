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

