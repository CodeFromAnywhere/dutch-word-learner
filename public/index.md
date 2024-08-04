Look at these examples. Make me a HTML file in the same style, that has the following functionality:

- A html that loads in `words.json` which is an array of strings of dutch words
- Ensure the user can enter and edit groq api key to save it in localStorage
- Every session, let the user select words from the array that the user doesn't know yet. Show 1 word RANDOMLY at a time. For each word, the user can say they know it, or they don't using a button. Keep going until the user selected 10 words they KNOW.
- put the words that are known in localStorage so they don't get selected next time.
- after the user has made their selection of all words, get all known words and call `getChatCompletion` with a prompt that goes: "Please make me a short Dutch story with these words: {words, comma separated}. Please ensure to not use any hard words in between, and have a lot of repetition.". add the result to a stories list in localStorage
- Below the session modal, show a list of all genearated prompts added to stories

```
async function getChatCompletion(prompt) {
  const apiKey = window.localStorage.getItem('apiKey'); // Replace with your actual API key
  const url = 'https://api.groq.com/openai/v1/chat/completions';

  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${apiKey}`
    },
    body: JSON.stringify({
      model: 'llama-3.1-70b-versatile',
      messages: [
        { role: 'user', content: prompt }
      ],
      temperature: 0.7
    })
  });

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  const data = await response.json();
  return data.choices[0].message.content;
}

// Example usage
getChatCompletion("Tell me a joke about programming")
  .then(result => console.log(result))
  .catch(error => console.error('Error:', error));
```
