---
title: "프로그래머스 기능 개발"     
date: 2020-09-30 13:20:28 -0400
categories: 알고리즘
---
# 문제
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

제한 사항
- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

**입출력 예**

|progresses|speeds|return|
|------|-----|---|		
|[93, 30, 55]|[1, 30, 5]|[2, 1]|
|[95, 90, 99, 99, 80, 99]|[1, 1, 1, 1, 1, 1]|[1, 3, 2]

## 입출력 예 #1
- 첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.
- 두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.
- 세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.

**따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.**

## 입출력 예 #2
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.

따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.


## 문제 요약
기능 개발에는 순서가 정해져있는데 먼저 끝내야할 기능보다 나중에 끝내도 될 기능이 먼저 개발된다면
배포는 먼저끝내야할 기능이 다 완성될 때 같이 배포됩니다.
배포될 때 몇 개의 기능이 배포될 지 구하시오!


## 풀이과정
- workDay 는 며칠이 걸려야 기능을 모두 완성하는 지 걸리는 일 수 입니다.
그걸 queue 에 담아주고, queue 에서 꺼낸 두 기능일수를 비교해줍니다.
먼저 들어간 기능의 일수가 더 많거나 같으면 배포할 때 수 (numOfDeploy) 가 증가합니다
그게 아니라면 1 로 다시 초기화 해줍니다. 그리고 for 문에서 다음 기능일 수를 비교하기 위해
최근 기능개발 일 수(currWorkDay)를 이전 기능개발 일 수 (prevWorkDay) 에 넣어줍니다.

```java
public int[] solution(int[] progresses, int[] speeds) {
    Queue<Integer> queue = new ConcurrentLinkedQueue();
    List<Integer> resultList = new ArrayList<>();
    for (int i = 0; i < progresses.length; i++) {
        int workDay = (int) Math.ceil((100 - progresses[i]) / speeds[i]);
        queue.add(workDay);
    }
    int prevWorkDay = queue.poll();
    int numOfDeploy = 1;
    while (!queue.isEmpty()) {
        int currWorkDay = queue.poll();
        if (prevWorkDay >= currWorkDay) {
            numOfDeploy++;
        } else {
            resultList.add(numOfDeploy);
            numOfDeploy = 1;
            prevWorkDay = currWorkDay;
        }
    }
    resultList.add(numOfDeploy);

    return resultList.stream().mapToInt(i -> i).toArray();
```


## 꼭 Queue 를 써야만 풀리는 문제일까?

그건 아닙니다! 단순히 for문 만 사용해서 문제 풀어보았습니다.

```java
public int[] solution2(int[] progresses, int[] speeds) {
    List<Integer> resultList = new ArrayList<>();
    int[] workdayArray = new int[progresses.length];
    for (int i = 0; i < progresses.length; i++) {
        int workDay = (int) Math.ceil((100 - progresses[i]) / speeds[i]);
        workdayArray[i] = workDay;
    }
    int prevWorkDay = workdayArray[0];
    int numOfDeploy = 1;
    for (int i = 1; i < workdayArray.length; i++) {
        int currWorkDay = workdayArray[i];
        if (prevWorkDay >= currWorkDay) {
            numOfDeploy++;
        } else {
            resultList.add(numOfDeploy);
            numOfDeploy = 1;
            prevWorkDay = currWorkDay;
        }
    }
    resultList.add(numOfDeploy);
    return resultList.stream().mapToInt(i -> i).toArray();
}
```