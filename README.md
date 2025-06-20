# clean-programming-with-gpt
클린코드, 리팩터링 원칙 등등을 gpt에게 입력 후 문제 만들어 달라 해보기 테스트   
참고 도서 Doit 클린 프로그래밍   
잘못된 예시는 X 표시   
옳게된 예시는 O 표시   

# 클린코드 원칙
## 1. 의미 있는 이름 짓기
### 네이밍 규칙 4가지
- 카멜 케이스: val name -> valName
- 파스칼 케이스: val name -> ValName
- 케밥 케이스: val name -> val-name
- 스네이크 케이스: val name -> val_name
#### 자바에서의 네이밍 규칙 (절대적인건 아님)
- 변수: 소문자 or 카멜 케이스
- 함수: 카멜 케이스
- 클래스: 파스칼 케이스
- 상수: 대문자
### 가독성 고려하기
작성한 의도를 예측 할 수 있는 코드
- X
``` java
public class test {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;

        System.out.println("sum: " + getA(a,b));
    }

    static public int getA(int a, int b) {
        return a + b;
    }
}
```
- O
``` java
public class Calculator {
    public static void main(String[] args) {
        int operand1 = 10;
        int operand2 = 20;

        System.out.println("sum: " + sumNumbers(operand1, operand2));
    }

    static public int sumNumbers(int operand1, int operand2) {
        return operand1 + operand2;
    }
}
```
### 기능에 맞는 이름 짓기
기능에 맞게 이름 짓기
- X
``` java
// Piece.java
public class Piece {
    String pieceColor;
    String pieceKind;
    GetPosition piecePosition;

    public Piece(String pieceColor, String pieceKind, GetPosition piecePosition) {
        this.pieceColor = pieceColor;
        this.pieceKind = pieceKind;
        this.piecePosition= piecePosition;
    }
}

// GetPosition.java
public class GetPosition {
    int x;
    int y;
    public GetPosition(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X() {
        return x;
    }

    public int Y() {
        return y;
    }

    public void setX(int x) {
        this.x = x;
    }

    public void setY(int y) {
        this.y = y;
    }
}

// Main.java
public class Main {
    static int[][] board = new int[8][8];

    public static void main(String[] args) {
        board[1][0] = 1;
        Piece piece = new Piece("white", "pawn", new GetPosition(0, 0));
        piecePosition(piece, 1);
        piecePosition(piece, 0);
        piecePosition(piece, 1);
    }

    static void piecePosition(Piece piece, int v) {
        int x = 0, y = 1;
        if (v == 0) {
            x = 1;
            y = 0;
        }

        if (board[piece.piecePosition.Y() + y][piece.piecePosition.X() + x] == 1) {
            System.out.println("킹을 죽였습니다.");
        } else {
            int m_x = piece.piecePosition.X() + x;
            int m_y = piece.piecePosition.Y() + y;

            System.out.println("[" + m_y + ", " + m_x + "]로 이동합니다.");
            piece.piecePosition.setX(m_x);
            piece.piecePosition.setY(m_y);
        }
    }
}
```
- O
``` java
// Piece.java
public class Piece {
    String color;
    String kind;
    Position position;

    public Piece(String color, String kind, Position position) {
        this.color = color;
        this.kind = kind;
        this.position= position;
    }
}

// Position.java
public class Position {
    int x;
    int y;
    public Position(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }
}

// Main.java
public class Main {
    static final int BOARD_SIZE = 8;
    static final int MOVE_DOWN = 1;
    static final int MOVE_RIGHT = 0;
    static final int ENEMY_KING_POSITION = 1;
    static int[][] board = new int[BOARD_SIZE][BOARD_SIZE];

    public static void main(String[] args) {
        board[1][0] = ENEMY_KING_POSITION;
        Piece piece = new Piece("white", "pawn", new Position(0, 0));
        movePiece(piece, MOVE_DOWN);
        movePiece(piece, MOVE_RIGHT);
        movePiece(piece, MOVE_DOWN);
    }

    static void movePiece(Piece piece, int direction) {
        int move_x = 0, move_y = 1;
        if (direction == MOVE_RIGHT) {
            move_x = 1;
            move_y = 0;
        }

        int next_position_x = piece.position.getX() + move_x;
        int next_position_y = piece.position.getY() + move_y;
        if (board[next_position_y][next_position_x] == ENEMY_KING_POSITION) {
            System.out.println("킹을 죽였습니다.");
        } else {
            System.out.println("[" + next_position_y + ", " + next_position_x + "]로 이동합니다.");
            piece.position.setX(next_position_x);
            piece.position.setY(next_position_y);
        }
    }
}
```
## 2. 주석 제대로 사용하기
### O 괜찮은 주석
#### 작성 의도를 설명하는 주석
코드만으로 의도한 바를 완벽히 나타내면 좋겠지만, 어려울 수 있다.
``` java
@Override
public int compareTo(Number o) { // 숫자 정렬 기준: 내림차순
	/**
	...
	*/
}
```
#### 정확한 정보나 의미를 알려 주는 주석
복잡한 정보를 다뤄야 할 수도 있다. 
``` java
// 이메일 정규식
public static String REGEXP_EMAIL = "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$";
```
#### TODO 주석
앞으로 처리해야 할 일이나 확인 사항등 기록
``` java
public Person(String name, int age, String gender) {
	this.name = name;
	this.age = age;
	this.gender = gender;
	// TODO. 추후 사는곳, 가족관계 추가
}
```
#### 강조 또는 경고를 나타내는 주석
강조
``` java
// 핸드폰 식별번호얻으므로 전화번호 앞 3자리만 잘라서 반환
static String getPhoneIdentificationNumber(String phoneNumber) {
	return phoneNumber.substring(0, 3);
}
```
#### 소유권 및 저작권을 명시한 주석
``` java
// Copyright (C) 2025 by shin. All rights reserved.

import java.util.Map;
```
### X 불필요한 주석
#### 작성 의도를 중복 설명하는 주석
코드 작성 의도를 변수나 함수 이름으로 충분히 알 수 있다면 굳이?
- X
``` java
public class Main {
    public static void main(String[] args) {
        int height = 170;
        int weight = 90;

        // 군대 현역 판단 조건문
        if (height > 150 && weight < 200) {
            System.out.println("현역입니다.");
        } else {
            System.out.println("면제입니다.");
        }
    }
}
```
- O
``` java
public class Main {
    public static void main(String[] args) {
        int height = 170;
        int weight = 90;
        if (heyManGoToArmy(height, weight)) {
            System.out.println("현역입니다.");
        } else {
            System.out.println("면제입니다.");
        }
    }
}
```
#### 데드 코드와 이력 관리용 주석
사용 안하는 코드는 찝찝해 하지 말고 삭제하자
``` java
// 2025-06-20 신제하 추가
public class Main {
    public static void main(String[] args) {
        int height = 170;
        int weight = 90;

        // 현역 조건 메서드를 작성하여서 주석처리
        // 혹시 모르니 주석
        // if (height > 150 && weight < 200) {
        if (heyManGoToArmy(height, weight)) {
            System.out.println("현역입니다.");
        } else {
            System.out.println("면제입니다.");
        }
    }
}
```
#### 거짓 정보가 담긴 주석
개발자들은 본능적으로 주석을 신뢰한다. 거짓이 있으면 안되지. 이거는 예시가 없어도 될 거 같다.
## 3. 복잡한 조건식은 함수로 변경하기
조건식이 복잡하면 직관적으로 이해하기 어렵다.
- X
``` java
// Score.java
public class Score {
    int korean;
    int english;
    int math;

    public Score(int korean, int english, int math) {
        this.korean = korean;
        this.english = english;
        this.math = math;
    }
}

Main.java
public class Main {
    public static void main(String[] args) {
        Score score = new Score(80, 70, 60);

        if (score.korean >= 70 && score.english >= 70 && score.math >= 70) {
            System.out.println("시험 통과");
        } else {
            System.out.println("탈락");
        }
    }
}
```
- O
``` java
// Score.java
public class Score {
    int korean;
    int english;
    int math;

    public Score(int korean, int english, int math) {
        this.korean = korean;
        this.english = english;
        this.math = math;
    }

    boolean isTestPass() {
        if (korean >= 70 && english >= 70 && math >= 70) {
            return true;
        } else {
            return false;
        }
    }
}

// Main.java
public class Main {
    public static void main(String[] args) {
        Score score = new Score(80, 70, 60);

        if (score.isTestPass()) {
            System.out.println("시험 통과");
        } else {
            System.out.println("탈락");
        }
    }
}
```
