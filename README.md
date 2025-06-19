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
