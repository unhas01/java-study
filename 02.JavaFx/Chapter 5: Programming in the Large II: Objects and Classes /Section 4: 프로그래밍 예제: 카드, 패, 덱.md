# Section 4: 프로그래밍 예제: 카드, 패, 덱

이 절에서는 영역 안에서 객체 지향 설계의 몇 가지 구체적인 예를 살펴보기로 한다. 여기서 설계는 상당한 재사용성을 지니는 무언가를 만들어 낼 수 있을 정도로 충분히 단순한 것이다. 표준적인 덱으로 이루어지는 카드 게임(이른바 "포커(poker)" 덱으로, 포커 게임에서 사용되는 덱)을 생각해보자.

## 1. 클래스 설계
전형적인 카드 게임에서 각 플레이어는 카드패를 받는다. 덱은 뒤섞이고 덱으로부터 카드는 한 번에 하나씩 뽑혀 플레이어 수중에 추가된다. 몇몇 게임에서는 손패에 있는 카드를 없앨 수 있고, 새로운 카드를 추가할 수 있다. 게임은 플레이어가 받은 카드의 값(에이스, 2, 3, ..., 킹) 및 문양(스페이드, 다이아몬드, 클럽, 하트)에 따라 승패가 갈린다. 이러한 설명에서 명사(noun)를 찾으면, 객체에 관한 여러 후보군이 있게 된다: 게임(game), 플레이어(player), 손패(hand), 카드(card), 덱(deck), 값(value), 문양(suit). 이들 중, 카드의 값과 문양은 단순한 값이며, Card 객체에서 인스턴스 변수로 나타낼 수 있다. 완전한 프로그램에서 나머지 5개의 명사들은 클래스로 나타낼 수 있다. 하지만 가장 명백하게 재사용성을 가지는 녀석들부터 다루도록 하자: 카드, 손패, 그리고 덱.

카드 게임의 설명에서 동사(verb)를 찾으면, 덱을 뒤섞은(shuffle) 다음 덱에서 카드 한 장을 뽑을(deal) 수 있다는 것을 알 수 있다. 이는 Deck 클래스에 관한 두 개의 인스턴스 메서드 후보군, 즉 `shuffle()`과 `dealCard()`를 제시한다. 카드는 손패에 추가하거나 손패에서 제거될 수 있다. 이는 Hand 클래스에 관한 두 개의 인스턴스 메서드 후보군을 제시한다: `addCard()`와 `removeCard()`. 카드는 비교적 수동적인 것이지만 적어도 그들의 문양과 값을 결정할 수 있어야 한다. 진행이 계속되면 더 많은 인스턴스 메서드를 발견하게 될 것이다.

우선, 덱 클래스를 상세하게 설계하자. 카드의 덱이 처음 생성되면, 이는 어떠한 표준적인 순서를 지닌 52장의 카드를 포함하고 있다. Deck 클래스는 새로운 덱을 생성하기 위한 생성자가 필요할 것이다. 어떤 새로운 덱도 다른 덱과 동일하기 때문에 생성자는 매개변수를 필요로 하지 않는다. 52장의 카드를 임의의 순서로 재정렬하는 `shuffle()`이라는 인스턴스 메서드가 있을 것이다. `dealCard()` 인스턴스 메서드는 다음 카드를 덱에서 가져올 것이다. 호출자는 어떠한 카드가 뽑히고 있는지 알 필요가 있기 때문에, 이 메서드는 `Card`라는 반환 자료형을 지닌 함수가 될 것이다. 이는 매개변수를 가지지 않는다 — 덱에서 다음 카드를 가져올 때, 덱에 어떤 정보도 제공하지 않는다; 단지 다음 카드가 무엇이든 간에 이를 얻을 뿐이다. `dealCard()` 메서드가 호출될 때 덱에 더 이상 카드가 없다면 어떤 일이 벌어질까? 빈 덱에서 카드를 가져오려고 하는 것은 아마도 오류로 간주되어야 하며, 따라서 그러한 경우에 덱은 예외를 던질 수 있다. 그러나 이로 인해 또 다른 의문이 제기된다: 프로그램의 나머지 부분은 어떻게 덱이 비어 있는지 알 수 있을까? 물론, 해당 프로그램은 얼마나 많은 카드를 사용했는지 추적할 수 있다. 그러나 덱 자체가 몇 장의 카드가 남았는지를 알아야 하기 때문에, 프로그램이 덱 객체에 물어 볼 수 있어야 한다. 덱에 남아 있는 카드의 갯수를 반환하는 다른 인스턴스 메서드인 `cardsLeft()`를 명시하여 이를 가능하게 할 수 있다. 이는 Deck 클래스의 모든 서브루틴에 대하여 완전한 명세로 이어진다:

덱(Deck) 클래스의 생성자와 인스턴스 메서드:

```java
     /**
      * 생성자. 섞이지 않은 카드 덱을 생성한다.
      */
     public Deck()

     /**
      * 사용된 모든 카드를 덱에 되돌린 다음,
      * 무작위한 순서로 이를 뒤섞는다.
      */
     public void shuffle()

     /**
      * 덱에서 카드가 뽑히면, 남은 카드의 갯수는 
      * 감소한다. 이 함수는 덱에 여전히 남아 있는
      * 카드의 갯수를 반환한다.
      */
     public int cardsLeft()

     /**
      * 덱에서 카드 하나를 뽑은 다음 이를 반환한다.
      * 남은 카드가 더 없다면 @throws IllegalStateException.
      */
     public Card dealCard()
```

덱 클래스를 **사용**하기 위해 알아야 할 것은 이것이 전부이다. 물론, 이는 어떻게 클래스를 작성하는지를 알려주지는 않는다. 이것은 코딩이 아니라 설계에 관한 연습이었다. 원한다면 소스 코드 Deck.java를 살펴볼 수 있다. 클래스에 인스턴스 변수로 Card 배열이 포함된 것은 놀랄 일이 아니지만, 이 시점에서는 이해할 수 없는 몇 가지 사항이 있을수도 있다. 물론, 그 구현에 대한 이해가 없더라도 프로그램에서 해당 클래스를 블랙박스로 사용할 수 있다.

비슷한 분석은 Hand 클래스에 대해서도 이루어질 수 있다. 손패의 객체가 처음 생성되면, 그 안에는 카드가 없다. **addCard()** 인스턴스 메서드는 손패에 카드를 추가할 것이다. 이 메서드는 추가할 카드를 지정하기 위해 Card 자료형인 매개변수를 필요로 한다. **removeCard()** 메서드의 경우에는 제거할 카드를 지정하기 위한 매개변수가 필요하다. 그러나 카드 자체를 지정해야 하는가("스페이드 에이스를 제거"), 아니면 손패에 있는 카드의 위치에 따라 카드를 지정해야 하는가("손패에 있는 세 번째 카드 제거")? 사실, 두 가지 선택지를 모두 허용할 수 있으므로 결정을 반드시 할 필요는 없다. 두 개의 **removeCard()** 인스턴스 메서드를 가지되, 하나는 제거되어야 할 카드를 지정하는 Card 자료형의 매개변수를 사용하고, 다른 하나는 손패에 있는 카드 위치를 지정하는 int 자료형의 매개변수를 사용하는 것이다. (클래스 안에서 동일한 이름을 가지는 두 개의 메서드가 존재할 수 있고, 이들은 서로 다른 숫자나 자료형의 매개변수를 가질 수 있다는 점을 기억하라.) 손패에는 다양한 수의 카드가 포함될 수 있기 때문에, 손패의 객체에게 얼마나 많은 카드를 가지고 있는지 물어 볼 수 있으면 편리할 것이다. 따라서, 손패에 있는 카드 갯수를 반환하는 인스턴스 메서드 **getCardCount()** 가 필요하다. 카드놀이를 할 때 필자는 같은 값의 카드가 서로 이웃하도록 손에 들고 있는 카드를 정리하는 것을 좋아한다. 이는 일반적으로 그렇게 할 수 있다면 유용한 일이기 때문에, 손패의 카드를 정렬하는 인스턴스 메서드를 제공할 수 있을 것이다. 재사용성을 갖춘 Hand 클래스에 대한 전체 명세는 다음과 같다:

손패(Hand) 클래스의 생성자와 인스턴스 메서드:

```java
    /**
     * 생성자. 최초 값은 비어 있는 Hand 객체를 생성.
     */
    public Hand()

    /**
     * 손패의 모든 카드를 버려서 손패를 비움.
     */
    public void clear()

    /**
     * 카드 c를 손패에 추가. c는 null이 아니여야 함.
     * c가 null이면 @throws NullPointerException.
     */
    public void addCard(Card c)

    /**
     * 지정된 카드가 손에 있다면, 이를 제거함.
     */
    public void removeCard(Card c)

    /**
     * 손패의 지정된 위치에 있는 카드를 제거함.
     * 카드는 0부터 번호를 셈.
     * 만약 지정된 위치가 손패에서 존재하지 
     * 않으면 @throws IllegalArgumentException.
     */
    public void removeCard(int position)

    /**
     * 손패의 카드 숫자를 반환.
     */
    public int getCardCount()

    /**
     * 손패의 주어진 위치에서 카드를 얻음.
     * 위치는 0부터 시작하여 번호를 셈.
     * 만약 지정된 위치가 손패에서 존재하지
     * 않으면 @throws IllegalArgumentException.
     */
    public Card getCard(int position)

    /**
     * 동일한 문양의 카드가 함께 그룹이 되도록, 
     * 그리고 문양 안에서 값에 의해 카드가 정렬이
     * 되도록 손패의 카드를 정렬.
     */
    public void sortBySuit()

    /**
     * 카드가 증가되는 값의 순서대로 정렬이 되도록
     * 손패의 카드를 정렬. 동일한 값을 가지는 카드는
     * 문양에 의해 정렬. 에이스가 가장 낮은
     * 값을 가지는 것으로 간주함.
     */
    public void sortByValue()
```

반복하지만, 이 시점에서 클래스의 구현에 관하여 이해하지 못할 몇 가지 것들이 있을 것이나, 이것이 프로젝트에서 해당 클래스의 사용을 그만두게 하진 않는다. 소스 코드는 Hand.java 파일에서 찾을 수 있다.

<hr>

## 2. 카드 클래스
Card 클래스의 설계와 구현에 대해 자세히 살펴보고자 한다. 해당 클래스는 생성되는 카드의 값과 문양을 지정하는 생성자를 갖는다. 문양은 네 가지로 되어 있으며, 정수 0, 1, 2, 3으로 나타낼 수 있다. 어떤 숫자가 어떤 문양을 나타내는지 기억하기에 어려울 것이므로, 필자는 네 가지 가능성을 나타내기 위해 Card 클래스에 명명된 상수(named constant)를 정의했다. 예를 들어 `Card.SPADES`는 문양 "스페이드"를 나타내는 상수다. (이 상수는 `public final static`인 `int`로 선언된다. 열거형을 사용하는 것이 더 나을 수도 있지만, 여기서는 정수 값인 상수를 고수할 것이다.) 카드의 가능한 값은 숫자 1, 2, ..., 13이며, 에이스(ace)는 1, 잭(jack)은 11, 퀸(queen)은 12, 킹(king)은 13이다. 반복하지만, 필자는 에이스와 얼굴형 카드(face card)의 값을 나타내기 위해 몇몇 명명된 상수를 정의했다. (Card 클래스를 읽으면 필자가 조커(joker)에 대한 지원 또한 추가했다는 점을 알게 될 것이다.)

Card 객체는 카드의 값과 문양을 안 채로 구성될 수 있다. 예를 들어 다음과 같은 문장으로 생성자를 호출할 수 있다:

```java
card1 = new Card( Card.ACE, Card.SPADES );  // 스페이드 에이스를 생성한다.
card2 = new Card( 10, Card.DIAMONDS );   // 다이아몬드 10을 생성한다.
card3 = new Card( v, s );  // 이는 허용된다. v와 s가 정수 표현식인
                            //               한에는 말이다.
```

Card 객체에는 해당 값과 문양을 나타내기 위한 인스턴스 변수가 필요하다. 필자는 이들을 클래스 밖에서 바꿀 수 없도록 `private`로 만들었고, 클래스 밖에서 해당 문양과 값을 발견할 수 있도록 `getSuit()` 및 ₩getValue()₩ 게터 메서드들을 제공하였다. 인스턴스 변수는 생성자에서 초기화되며 그 이후에는 절대로 변경되지 않는다. 사실, 필자는 인스턴스 변수들 `suit`와 `value`를 `final`로 선언했었는데, 이들은 초기화된 이후에 절대로 바뀌지 않기 때문이다. 인스턴스 변수는 해당 선언의 초기 값이 주어지거나 클래스의 모든 생성자에서 초기화되는 경우 `final`로 선언할 수 있다. 모든 해당 인스턴스 변수가 `final`이기 때문에 Card 는 불변적인(immutable) 객체다.

마지막으로, 사람이 읽을 수 있는 형태로 카드를 인쇄하기 쉽게 하기 위해 클래스에 몇 가지 편리한 메서드를 추가하였다. 예를 들면, 다이아몬드를 나타내기 위해 클래스에서 사용되는 무의미한 코드 번호 2가 아니라 "다이아몬드"라는 단어로 카드의 문양을 출력할 수 있기를 바란다. 이는 아마도 많은 프로그램에서 해야 할 일이기 때문에, 클래스에 이에 대한 지원을 포함하는 것이 타당하다. 그래서 필자는 카드의 문양 및 값의 문자열 표상을 반환하는 `getSuitAsString()` 및 `getValueAsString()` 인스턴스 메서드를 제공했다. 마지막으로 "하트 퀸"과 같이 값과 문양을 모두 갖춘 문자열을 반환하기 위한 `toString()` 인스턴스 메서드를 정의했다. 이 메서드는 카드를 + 연산자를 통해 문자열로 연결하는 경우와 같이 Card 가 String 으로 변환되어야 할 때마다 자동적으로 사용될 것이라는 점을 떠올려라. 따라서, 다음 문장

```java
System.out.println( "당신의 카드는 " + card );
```

은 다음과 같다:

```java
System.out.println( "당신의 카드는 " + card.toString() );
```

만약 카드가 하트 퀸이라면, 이들 중 어느 것이든 "당신의 카드는 하트 퀸"이라고 출력할 것이다.

Card.java에서도 찾아볼 수 있는 완전한 Card 클래스가 있다. 이 클래스는 높은 재사용성을 가지기에 충분할 정도로 일반적이기 때문에, 이를 설계, 작성, 테스트한 것에 투입된 노력이 장기적인 성과를 거두고 있다.

```java
/**
* Card 자료형인 객체는 표준 포커 덱으로 카드 게임을 하는 것을
* 나타내며, 조커를 포함한다. 카드는 문양을 지니며, 이는
* 스페이드, 하트, 다이아몬드, 클럽, 또는 조커일 수 있다.
* 스페이드, 하트, 다이아몬드, 또는 클럽은 13가지의 값 중 하나를 가진다:
* 에이스, 2, 3, 4, 5, 6, 7, 8, 9, 10, 잭, 퀸, 또는 킹. "에이스"는
* 가장 작은 값으로 간주됨에 유의하라. 조커는 또한 관련 값을 가질 수 있다;
* 이 값은 어떤 것도 될 수 있으며 여러 개의 다른 조커들을
* 추적하는 데 사용될 수 있다.
  */
public class Card {

    public final static int SPADES = 0;   // 4가지 문양에 조커를 추가한 것에 대한 코드.
    public final static int HEARTS = 1;
    public final static int DIAMONDS = 2;
    public final static int CLUBS = 3;
    public final static int JOKER = 4;
    public final static int ACE = 1;      // 숫자가 아닌 카드에 대한 코드.
    public final static int JACK = 11;    //   2부터 10 사이의 카드는
    public final static int QUEEN = 12;   //   이들에 대한 코드로 숫자 값을 가진다.
    public final static int KING = 13;

  /**
    * 해당 카드의 문양으로, 상수 SPADES, HEARTS, DIAMONDS,
    * CLUBS, 또는 JOKER 중 하나이다. 해당 카드가 생성된 이후에는
    * 문양은 변경될 수 없다.
      */
    private final int suit;

  /**
    * 해당 카드의 값. 평범한 카드의 경우 이는 1부터 13 사이의 값 중 하나이며,
    * 여기서 1은 ACE를 나타낸다. JOKER의 경우 그 값은 무엇이든 될 수 있다.
    * 해당 카드가 생성된 이후에는 값은 변경될 수 없다.
      */
    private final int value;

  /**
    * 관련 값으로 1을 가지는 조커를 생성한다.
    * "new Card()"는 "new Card(1,Card.JOKER)"와 동일함에 유의하라.)
      */
    public Card() {
        suit = JOKER;
        value = 1;
    }

  /**
    * 지정된 문양과 값으로 카드를 생성한다.
    * @param theValue 새로운 카드의 해당 값. 일반적인 카드(조커가 아닌)의 경우,
    * 해당 값은 반드시 1부터 13 사이의 범위에 있어야 하며, 여기서 1은 에이스를 나타낸다.
    * 상수 Card.ACE, Card.JACK, Card.QUEEN, 및 Card.KING을 사용할 수도 있다.
    * 조커의 경우, 해당 값은 무엇이든 될 수 있다.
    * @param theSuit 새로운 카드의 해당 문양. 이는 Card.SPADES, Card.HEARTS,
    * Card.DIAMONDS, Card.CLUBS, 또는 Card.JOKER 값들 중 반드시 하나여야 한다.
    * @throws IllegalArgumentException 매개변수 값이 허용되는 범위 안에
    * 있지 않은 경우
      */
    public Card(int theValue, int theSuit) {
        if (theSuit != SPADES && theSuit != HEARTS && theSuit != DIAMONDS &&
            theSuit != CLUBS && theSuit != JOKER)
            throw new IllegalArgumentException("허용되지 않는 카드 게임 문양");
        if (theSuit != JOKER && (theValue < 1 || theValue > 13))
            throw new IllegalArgumentException("허용되지 않는 카드 게임 값");
      value = theValue;
      suit = theSuit;
    }

  /**
    * 이 카드의 문양을 반환.
    * @return 문양은 상수 Card.SPADES,
    * Card.HEARTS, Card.DIAMONDS, Card.CLUBS, 또는 Card.JOKER 중 하나임.
      */
    public int getSuit() {
        return suit;
    }

  /**
    * 이 카드의 값을 반환.
    * @return 값은 평범한 카드의 경우 숫자 1부터 13 사이의 것 중 하나이며
    * 조커의 경우 어떤 값도 될 수 있음.
      */
    public int getValue() {
        return value;
    }

  /**
    * 카드의 문양에 관한 문자열 표상을 반환.
    * @return 문자열 "스페이드", "하트", "다이아몬드",
    * "클럽" 또는 "조커" 중 하나임.
      */
    public String getSuitAsString() {
        switch ( suit ) {
        case SPADES:   return "스페이드";
        case HEARTS:   return "하트";
        case DIAMONDS: return "다이아몬드";
        case CLUBS:    return "클럽";
        default:       return "조커";
        }
    }

  /**
    * 카드의 값에 관한 문자열 표상을 반환.
    * @return 평범한 카드는 문자열 "에이스", "2",
    * "3", ..., "10", "잭", "퀸", 또는 "킹" 중 하나를 반환한다.
    * 조커의 경우에는 문자열은 언제나 숫자다.
      */
    public String getValueAsString() {
        if (suit == JOKER)
            return "" + value;
        else {
            switch ( value ) {
            case 1:   return "에이스";
            case 2:   return "2";
            case 3:   return "3";
            case 4:   return "4";
            case 5:   return "5";
            case 6:   return "6";
            case 7:   return "7";
            case 8:   return "8";
            case 9:   return "9";
            case 10:  return "10";
            case 11:  return "잭";
            case 12:  return "퀸";
            default:  return "킹";
            }
        }
    }

  /**
    * 이 카드의 문양 및 값을 포함하는 문자열 표상을 반환
    * (예외적으로 값 1을 가지는 조커의 경우 반환되는 값은
    * 단순히 "조커"임). 샘플 반환 값은 다음과 같음:
    * "하트 퀸", "다이아몬드 10", "스페이드 에이스",
    * "조커", "조커 #2"
      */
    public String toString() {
        if (suit == JOKER) {
        if (value == 1)
            return "조커";
        else
            return "조커 #" + value;
        }
        else
            return getValueAsString() + " " + getSuitAsString();
    }
    
} // 클래스 Card 종료
```
<hr>

## 3. 예제: 간단한 카드 게임
Card 및 Deck 클래스를 사용하는 완전한 프로그램을 소개하면서 이 절을 마치고자 한다. 해당 프로그램은 사용자가 하이로우(HighLow)라고 불리는 매우 간단한 카드 게임을 할 수 있도록 한다. 덱을 뒤섞은 후 카드 한 장을 덱에서 뽑아 사용자에게 보여준다. 사용자는 덱에서 나올 다음 카드가 현재 카드보다 높은지 낮은지를 예상한다. 사용자가 정확하게 예상하면 덱의 다음 카드가 현재 카드가 되고, 사용자는 또 다른 예상을 한다. 이는 사용자가 틀린 예측을 할 때까지 계속된다. 정확하게 예측한 횟수는 사용자의 점수이다.

필자의 프로그램은 한 번의 하이로우 게임을 수행하는 정적 메서드를 가지고 있다. `main()` 루틴은 사용자가 하이로우 게임을 여러 번 할 수 있도록 한다. 마지막에는 사용자의 평균 점수를 보고한다.

이 프로그램에서 사용되는 알고리즘의 개발에 대하여 다루지는 않겠지만, 주의 깊게 읽어보고 어떻게 작동하는지 확실히 이해해주길 권하겠다. 특히 한 번의 하이로우 게임을 수행하는 서브루틴은 게임에서의 사용자 점수를 반환 값으로 돌려준다는 점에 유의하라. 이는 점수를 필요로 하는 메인 프로그램으로 이를 돌려준다. 해당 프로그램은 다음과 같다:

```java
import textio.TextIO;

/**
* 이 프로그램은 사용자가 하이로우(HighLow)라는 간단한 카드 게임을 할 수
* 있도록 하며, 이는 main() 루틴의 시작에 있는 출력문에 설명되어 있다.
* 사용자가 여러 번의 게임을 한 뒤에는, 사용자의 평균 점수가
* 보고될 것이다.
  */
  public class HighLow {


public static void main(String[] args) {

      System.out.println("이 프로그램은 간단한 카드 게임, 하이로우(HighLow)를");
      System.out.println("당신이 할 수 있도록 합니다. 카드는 덱에서 뽑힐 것입니다.");
      System.out.println("다음 카드가 더 높은지(higher) 낮은지(lower)를");
      System.out.println("예상해야 합니다. 게임의 점수는 틀린 예상을 하기");
      System.out.println("전까지 이루어진 정확한 예상의 횟수입니다.");
      System.out.println();

      int gamesPlayed = 0;     // 사용자가 플레이 한 게임의 횟수.
      int sumOfScores = 0;     // 플레이 된 모든 게임의 점수들의 합
      double averageScore;     // 평균 점수, sumOfScores를
                               //      gamesPlayed로 나누어 계산.
      boolean playAgain;       // 사용자가 다른 게임을 하길 원하는지 
                               //   여부에 대한 응답을 기록

      do {
         int scoreThisGame;        // 한 번의 게임에 대한 점수.
         scoreThisGame = play();   // 게임을 실행하고 점수를 얻음.
         sumOfScores += scoreThisGame;
         gamesPlayed++;
         System.out.print("게임을 다시 합니까? ");
         playAgain = TextIO.getlnBoolean();
      } while (playAgain);

      averageScore = ((double)sumOfScores) / gamesPlayed;

      System.out.println();
      System.out.println("당신은 " + gamesPlayed + " 번의 게임을 하였습니다.");
      System.out.printf("당신의 평균 점수는 %1.3f.\n", averageScore);

}  // main() 종료


/**
* 사용자가 하이로우(HighLow) 게임 한 번을 하도록 하며 해당
* 게임의 득점을 반환한다. 점수는 사용자가 맞게
* 추측한 횟수를 의미한다.
*/
private static int play() {

      Deck deck = new Deck();  // 새로운 카드의 덱을 얻고 
                               //   변수 deck에 이에 대한 참조를 저장한다.

      Card currentCard;  // 사용자가 보는 현재의 카드.

      Card nextCard;   // 덱에 있는 다음 차례의 카드. 사용자는 
                       //    이것이 현재의 카드보다 높은지 아니면
                       //    낮은지를 예측하려 할 것이다.

      int correctGuesses ;  // 사용자가 한 정확한 예측의 횟수.
                            //   게임의 끝에 이르면 이는 사용자의
                            //   점수가 될 것이다.

      char guess;   // 사용자의 추측. 다음 차례의 카드가 높다면 'H',
                    //   다음 차례의 카드가 낮다면 'L'이 된다.

      deck.shuffle();  // 게임을 시작하기 전에 덱을 무작위한 순서로
                       //    뒤섞는다.

      correctGuesses = 0;
      currentCard = deck.dealCard();
      System.out.println("첫 번째 카드는 " + currentCard);

      while (true) {  // 사용자의 예측이 틀리면 루프를 종료.

         /* 사용자의 예측 'H' 또는 'L'('h' 또는 'l'도 가능)을 얻는다. */

         System.out.print("다음 차례의 카드는 높을까요 (H) 아니면 낮을까요 (L)?  ");
         do {
             guess = TextIO.getlnChar();
             guess = Character.toUpperCase(guess);
             if (guess != 'H' && guess != 'L') 
                System.out.print("H 또는 L로 대답해주세요:  ");
         } while (guess != 'H' && guess != 'L');

         /* 다음 차례의 카드를 얻어서 사용자에게 보여준다. */

         nextCard = deck.dealCard();
         System.out.println("다음 차례의 카드는 " + nextCard);

         /* 사용자의 예측을 얻는다. */

         if (nextCard.getValue() == currentCard.getValue()) {
            System.out.println("이전의 카드와 그 값이 일치합니다.");
            System.out.println("동률로 게임을 졌습니다. 유감이네요!");
            break;  // 게임 종료.
         }
         else if (nextCard.getValue() > currentCard.getValue()) {
            if (guess == 'H') {
                System.out.println("예측이 맞았습니다.");
                correctGuesses++;
            }
            else {
                System.out.println("예측이 틀렸습니다.");
                break;  // 게임 종료.
            }
         }
         else {  // nextCard is lower
            if (guess == 'L') {
                System.out.println("예측이 맞았습니다.");
                correctGuesses++;
            }
            else {
                System.out.println("예측이 틀렸습니다.");
                break;  // 게임 종료.
            }
         }

         /* 루프의 다음 번 반복을 설정하기 위하여, nextCard는 
            currentCard가 된다. currentCard는 사용자가 보는 카드가 되어야 하며, 사용자가 예측을 한 후 nextCard는 덱의 다음 카드로 설정될 것이기 때문이다. */

         currentCard = nextCard;
         System.out.println();
         System.out.println("카드는 " + currentCard);

      } // while 루프 종료

      System.out.println();
      System.out.println("게임이 끝났습니다.");
      System.out.println("당신은 " + correctGuesses 
                                           + " 번의 맞는 예측을 하였습니다.");
      System.out.println();

      return correctGuesses;

    }  // play() 종료


} // 클래스 HighLow 종료
```
