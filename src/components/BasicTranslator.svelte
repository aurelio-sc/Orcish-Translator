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
        //console.log(wordsModified[i] + " apóstrofe!");
        wordsModified[i]=wordsModified[i].substring(0, wordsModified[i].length - 2); //the word loses the 'S.
        wordsModified.splice(i+1, 0, lastTwo); //" or ' added to the array after the word.        
        i+=1;
      };
    };

    console.log(wordsModified);

    const dictionary_url = 'https://api.jsonbin.io/b/622a023406182767437313db/2';
    
    async function getData(){
      const response = await fetch(dictionary_url);
      const dictionary_json = await response.json();
      //console.log(dictionary_json);
    
      for (let i=0; i< wordsModified.length; i++) { //Translate to orcish
        if (Object.keys(dictionary_json).includes(wordsModified[i])){
          console.log(wordsModified[i] + " encontrada no dicionário!");
          wordsModified[i]=dictionary_json[wordsModified[i]];
        };
      
      };
      console.log(wordsModified);
      
      for (let i=0; i<wordsModified.length-1; i++){ //PAROU DE FUNCIONAR!?
        wordsModified[i] = wordsModified[i].toLowerCase();
        if (punctuation.includes(wordsModified[i])){
          wordsModified[i] = wordsModified[i];
          wordsModified[i+1] = wordsModified[i+1].charAt(0).toUpperCase() + wordsModified[i+1].slice(1); 
          console.log(wordsModified[i+1] + " vem após pontuação!")         
        }else{
          wordsModified[i] = " " + wordsModified[i];          
        }
        translatedText = wordsModified.join("");
      }

      

    };
    getData();
  

  };

 
 


</script>
<div class="container">

  <p class="translatedText">{translatedText}</p>

  <textarea 
    bind:value={text}
    class="textArea"
    cols="50"
    rows="10"/>

  <button on:click={translate} class="translate">T r a n s l a t e</button>

</div>

<style>

  .container{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 1rem;
    margin-top: 2rem;
  }
  .textArea{
    resize: none;
    border-radius: 0.75rem;
    padding: 1rem;
    font-family: 'Source Code Pro';
    font-weight: bold;
  }

  .translate {
    height: 2rem;
    background-color: #CECECE;
    border:0.0125rem solid;
    border-radius: 0.25rem;
    font-weight: bold;
  }

  .translate:hover {
    background-color: #BDBDBD;
  }

  .translate:active {
    background-color: #ACACAC;
  }

  .translatedText{
    font-family: 'Rock Salt';
    font-weight: bold;
  }

</style>