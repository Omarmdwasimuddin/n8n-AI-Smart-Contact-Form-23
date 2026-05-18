## n8n-AI-Smart-Contact-Form

[See Supabase Integration](https://github.com/Omarmdwasimuddin/n8n-Database-22)

#### click: Add first step--->search & click: webhook--->HTTP Method e daw: POST--->Path e daw: contact-form--->copy koro test url

#### vs code editor e index.html er modhe test url paste koro.--->click koro: Execute workflow--->index.html file ke open to live kore---> data diye Send Message button click koro. 

#### webhook er + sign click koro--->search & click koro: open ai--->click: Message a model--->click koro: Set up credential--->visit koro: https://platform.openai.com/home [login/signup korte hobe]--->create api key click koro--->Name diye create secret key button click kore api key copy koro--->credential e paste kore save kore daw.--->Model e daw: GPT-4O-MINI--->Prompt e daw: prompt.txt er modhe thaka prompt gulo daw [prompt er modhe message er pashe {{ $json.body.message }} daw]--->Add option click koro--->select koro: Output Format--->Type e daw: Json Object--->Execute step click koro.

#### click koro Message a model er +sign--->search & click: supabase--->click: Create a row--->supabase dashboard e jaw--->supabase er SQL Editor tab e jaw--->query.sql paste koro--->click koro: run--->Create a row er modhe data set korbo[pash theke value ene drag & drop kore dibo]--->click: execute step

#### click koro: Create a row er + sign--->search & click koro: if--->condition set koro[priority high hole message send hobe]--->if er +sign click koro--->search & click: gmail--->click: send a message--->value set koro.
