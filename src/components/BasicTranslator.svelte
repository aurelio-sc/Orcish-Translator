<script>
   /*
    TODO:
    [X] Checar se a palavra é seguinda de pontuação e fazer a separação em [..., palavra , pontuação , ...] 
    [X] Checar se a palavra termina com ' ou " e fazer a separação em [..., ' , palavra , ' , ...].
    [X] Checar se os dois últimos caracteres são 's e fazer a separação [..., palavra, 's , ...].
    [ ] No segundo check (de ' e ") só precisa começar ou terminar com um dos dois caracteres. Mudar isso.
    [ ] Checar se a palavra está no dicionário (JSON) e fazer a substituição.
    */

  let text='';
  let translatedText='';
  const punctuation=[".", ",", ";", ":", "!", "?"];
  
  function translate(){
    translatedText=text;
    //console.log(translatedText);
    let words = text.toUpperCase().split(" "); //words is an array of uppercased words. still with ", ', punctuation, etc.
    let wordsModified=words;
    //console.log(words);

    //CHECKING FOR PUNCTUATION:
    for (let i=0;i<wordsModified.length;i++) {
      let punc = wordsModified[i].slice(-1);
      if ( punctuation.includes(punc) ) { //checks if the word has punctuation.
        //console.log(wordsModified[i] + " tem pontuação!");
        wordsModified[i]=wordsModified[i].substring(0, wordsModified[i].length - 1); //the word loses its punctuation.
        wordsModified.splice(i+1, 0, punc); //punctuation added to the array after the word.
        i++;
      };
    };
    //console.log(wordsModified);
   
    
    //CHECKING FOR ' AND ":
    for (let i=0;i<wordsModified.length;i++) {
      let first = wordsModified[i].slice(0,1);
      let last = wordsModified[i].slice(-1);
      //console.log(first + " and " + last);
      if ( first == last && (first=="'" || first == "\"") ) { //checks if the word starts and ends with either " or '.
        //console.log(wordsModified[i] + " tem aspas!");
        wordsModified[i]=wordsModified[i].substring(1, wordsModified[i].length - 1); //the word loses both ' or ".
        wordsModified.splice(i, 0, first); //" or ' added to the array before the word.
        wordsModified.splice(i+2, 0, last); //" or ' added to the array after the word.        
        i+=2;
      };
    };
    
    //console.log(wordsModified);


    //CHECKING FOR 'S:
    for (let i=0;i<wordsModified.length;i++) {
      let lastTwo = wordsModified[i].slice(-2);
      //console.log(lastTwo);
      if ( lastTwo == "'S") { //checks if the word starts and ends with 'S.
        console.log(wordsModified[i] + " apóstrofe!");
        wordsModified[i]=wordsModified[i].substring(0, wordsModified[i].length - 2); //the word loses the 'S.
        wordsModified.splice(i+1, 0, lastTwo); //" or ' added to the array after the word.        
        i+=1;
      };
    };

    console.log(wordsModified);

  };


</script>
<div class="container">

  <p>{translatedText}</p>

  <textarea 
    bind:value={text}
    class="textArea"
    cols="50"
    rows="10"/>

  <button on:click={translate}>Translate</button>

</div>

<style>

  .container{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 1rem;
  }
  .textArea{
    resize: none;
  }
</style>