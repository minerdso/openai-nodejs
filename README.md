# openai-nodejs
Usando openai no seu bot WhatsApp/baileys



#codigo simples que eu fiz para adaptar no meu bot.

Depdencias: openai - dotenv - axios

# Declara no seu codigo principal


require('dotenv').config();
const { Configuration, OpenAIApi } = require('openai');

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);


#criei essa funçao para chamar ela na case

async function getChatGPTResponse(prompt) {
  if (!configuration.apiKey) {
    return "KEY NAO CONFIGURADA";
  }

  try {
    const completion = await openai.createCompletion({
              model: "text-davinci-003",
        prompt: `Eu sou um bot de perguntas e respostas altamente inteligente. Se você me fizer uma pergunta baseada na verdade, eu lhe darei a resposta. Se você me fizer uma pergunta que seja sem sentido, truque ou não tenha uma resposta clara, eu responderei com "Desconhecido".\n\nQ: ${prompt}\nA:`,
        temperature: 0.5,
        max_tokens: 1024,
        top_p: 1,
        frequency_penalty: 0.0,
        presence_penalty: 0.0,
        stop: ["\n"],
      });
    return completion.data.choices[0].text;
  } catch (error) {
    // Consider adjusting the error handling logic for your use case
    if (error.response) {
      console.error(error.response.status, error.response.data);
      return error.response.data;
    } else {
      console.error(`Error with OpenAI API request: ${error.message}`);
      return "An error occurred during your request.";
    }
  }
}


# modo de chamar ela 


switch (command) {

 
  case 'ai':
   
    const promptAi = args.join(' ');
    const chatGPTResponse = await getChatGPTResponse(promptAi);
    conn.sendMessage(from, { text: chatGPTResponse });
    break; 
    
    
    ex: /ai tudo bem
    
    
   #tio minerd 
    

