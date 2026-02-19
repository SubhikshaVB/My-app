# CEG Day 2 workshop

# Step 1

use the template and run the code https://www.npmjs.com/package/create-ceg-day2-starter

get Gemini API Key and fill it in `.env.local`

## Step 2

npm run dev

you will have a basic ui running

# step 3

reading markdown

```
npm install @uiw/react-markdown-preview --save
```

# Step 4

replace {text} in ChatMesssage.tsx with

```
 <MarkdownPreview source={text} style={{ padding: 16 }} />       
```

# Step 5

`TASK 1` : fill the form : Think of some use case (your college specific problem) and solve it using this chatbot , the problem should be college specific , and create a robust system prompt to make the llm solve the problem following the guidelines 

https://docs.google.com/forms/d/e/1FAIpQLSfp_4YiuMSzbpxH9UYzpV61XJupNYvC0B0DU27Dl2hlhOBVhg/viewform?usp=dialog

fill the form 

![Screenshot 2026-02-19 at 12.26.04 PM.png](attachment:667642cd-472b-42f1-acf4-18bf898d55ae:Screenshot_2026-02-19_at_12.26.04_PM.png)

## Step5.1

Grouping the responses , 

- category of solution (academic , mess , hostel , operation)
- based on usergroup
- severity

[Insights](https://www.notion.so/Insights-30c39ce3bee08052a852c9ead8a91733?pvs=21)

meetlink : https://meet.google.com/fvh-fpvi-jkk 

join the meet to view the screen

```
import { streamText, UIMessage, convertToModelMessages } from 'ai';
import { google } from '@ai-sdk/google';
import { tools } from './tools'; 

export async function POST(req: Request) {
  const { messages }: { messages: UIMessage[] } = await req.json();
  console.log("Messages are ",messages)
  //TODO TASK 1
  const context=`
  we have two main entry gates for CEG 
  1) kotturpuram entry
  2) main gate entry

  Timings of the college 
  8:30am-4:30pm

  `
  const systemPrompt = `You are a security person for CEG guindy , 
  you stop people and ask them why they are here , and also help them with details
  always be crisp, only 2 sentences at max
  following is the context:
  ${context}
  `;

  const result = streamText({
    model: google('gemini-2.5-flash'),
    system: systemPrompt,
    messages: await convertToModelMessages(messages),

    //TODO TASK 2 - Tool Calling
    // tools,            // Uncomment to enable tool calling
    // maxSteps: 5,      // Allow multi-step tool use (model calls tool → gets result → responds)
  });

  return result.toUIMessageStreamResponse();
}

```

# Step 6

deploying the chatbot

- create a GitHub repository
    
    ![Screenshot 2026-02-19 at 2.32.05 PM.png](attachment:e03f0436-6c4e-4822-af9a-5416093bca86:Screenshot_2026-02-19_at_2.32.05_PM.png)
    

lets resolve build errors

update next.config.ts wiht 

```
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  /* config options here */
  typescript: {
    ignoreBuildErrors: true,
  },
};

export default nextConfig;

```

build the project , see if build is successful

```
npm run build
```

push the project to GitHub

```
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin <repo link>
git push -u origin main
```

# lets deploy this code in vercel

open vercel.com

![Screenshot 2026-02-19 at 2.58.39 PM.png](attachment:c14c95d6-35ea-4548-bdbd-939634f4c1fe:Screenshot_2026-02-19_at_2.58.39_PM.png)

create a new project by clicking `Add new`

authorise and select your newly created repo
