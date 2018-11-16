# Control Flow

## Control Transfer Statements

### Labeled Statements

Swift 에서 반복문과 조건문을 다른 반복문과 조건문 내부에 포함하여 복잠한 제어 흐름 구조를 만들 수 있다. 그러나 반복문과 조건문은 둘 다 break 문을 사용하여 실행을 중간에 종료 시킬 수 있다. 따라서 구분문을 종료하려는 반복문 또는 조건문을 명확히 하는 것이 유용할 수 있다. 중첩된 반복문이 여러개 있는 경우 어떤 반복문에게 영향을 갈 것인가를 명시적으로 지정할 수 있다. 

이러한 목적을 달성하기 위해서 반복문이나 조건문에 `statement label` 로 표시할 수 있다. 조건문에 `statement label`을 달고 해당 label 이 달려있는 조건문이 끝나는 부분에 `break`를 실행할 수 있다. 반복문에는 `statement label`를 표시하여 실행되는 마지막에 `break` 나 `continue` 을 사용할 수 있다. 

`statement label` 는 구문의 시작 부분과 같은 라인에 `:` 함께 사용한다. 모든 반복문과 switch 문과 원리는 같지만 `while`에 사용하는 문법 예제는 다음과 같다:

`label name`: while `condition` {  
	`statements`  
}  



```swift
gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // diceRoll will move us to the final square, so the game is over
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // diceRoll will move us beyond the final square, so roll again
        continue gameLoop
    default:
        // this is a valid move, so find out its effect
        square += diceRoll
        square += board[square]
    }
}
print("Game over!")
```


