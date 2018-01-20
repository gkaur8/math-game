# math-game
var maxNum = 12,
    signWidt = 12,
    question = document.getElementById( 'question' ),
    input = document.getElementById( 'input' ),
    errorMessage = document.getElementById( 'errorMessage' ),
    greatMessage = document.getElementById( 'greatMessage' ),
    answer;
//Some of these signs are closer to what children use in school
var plusSign = '+',
    minusSign = '\u2212',
    timesSign = '\u2212',
    divideSign = '\xF7',
    signs = [plusSign , minusSign, timesSign, divideSign];


function randInt(max) {
  return Math.floor(Math.random() * max);
}

function calculateAnswer(n1, sign, n2) {
  switch (sign) {
    case plusSign:
      return n1 + n2;
    case minusSign:
      return n1 - n2;
    case timesSign:
      return n1 * n2;
    case divideSign:
      return n1 / n2;
    default:
      return false;
  }
}

function displayQuestion( s ){
  //We know the question will never surpass 12 chars..
  var extraneousSpace = 12 - s.length, spaceLeft, spaceRight;
  //Compare last bit to 1, if that bit is 1, the number is odd
  //So we subtract 1 before we evenly divide by 2 and 1 space on the right
  if( extraneousSpace & 1 ){
    extraneousSpace--;
    spaceLeft = extraneousSpace / 2;
    spaceRight = spaceLeft + 1;
  } else {
    spaceLeft = spaceRight = extraneousSpace / 2;
  }
  //Mildly evil re-use of variable and magical space repeating routine
  //http://stackoverflow.com/questions/202605/repeat-string-javascript
  spaceLeft = new Array( spaceLeft + 1 ).join( ' ' );
  spaceRight = new Array( spaceRight + 1 ).join( ' ' );
  question.innerText = spaceLeft + s + spaceRight;
}

function generateQuestion(){
  var sign = signs[randInt(4)],
      num1 = randInt(maxNum + 1),
      num2 = randInt(maxNum + 1);  
  
  if (sign == minusSign){
    num1 += num2;
  }
  if (sign == divideSign) {
    num2 = randInt(maxNum) + 1;
    num1 *= num2;
  }  
  answer = calculateAnswer( num1 , sign, num2 );
  displayQuestion( num1 + ' ' + sign + ' ' + num2 )
}

generateQuestion();
input.focus();

input.addEventListener( 'keypress' , function(e){
  console.log( this );
  if( e.which == 13 || e.keyCode == 13 ){
    if( input.value != answer ){
      errorMessage.innerText = 'Wrong, ' + question.innerText.trim() + ' = ' + answer;
      greatMessage.innerText = '';
    }else {
      errorMessage.innerText = '';
      greatMessage.innerText = 'That is right, here\'s another question';    
    }
    input.value = '';
    generateQuestion();
  }
});

#errorMessage { color: red }
#greatMessage { color: green }
<pre>
|￣￣￣￣￣￣ |
|   What is  |
|<span id='question'>            </span>| 
|      ?     |
| ＿＿＿＿＿__| 
(\__/) || 
(•ㅅ•) || 
/ 　 づ
</pre>
<input type='text' id='input'><br>
<pre>
<span id='errorMessage'></span><span id='greatMessage'></span>
</pre>
