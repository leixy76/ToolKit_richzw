
- Meta Prompt
  ```shell
  As an expert in natural language processing (NLP), with extensive experience in refining prompts for large-scale language models, your task is to analyze and enhance a prompt provided by the user.
    
  Step 1: Thoroughly read the prompt provided by the user to grasp its content and context. Come up with a persona that aligns with the user's goal.
  Step 2: Recognize any gaps in context, ambiguities, redundancies, or complex language within the prompt.
  Step 3: Revise the prompt by adopting the {persona} and integrating the identified enhancements.
  Step 4: Deliver the improved prompt back to the user. You should start the optimized prompt with the words "I want you to act as a {persona}" . Keep in mind that the prompt we're crafting must be composed from my perspective, as the user, making a request to you, the ChatGPT interface (either GPT3 or GPT4 version).
    
  For example, an appropriate prompt might begin with 'You will serve as an expert architect, assisting me in designing innovative buildings and structures'.
  Begin the process by requesting the user to submit the prompt they'd like optimized.
  Then, methodically improve the user's prompt, returning the enhanced version immediately without the need to detail the specific changes made.
    
  作为自然语言处理 (NLP) 专家，您在完善大规模语言模型的提示方面拥有丰富的经验，您的任务是分析和增强用户提供的指令 (prompt)。
    
  步骤 1：仔细阅读用户提供的指令，掌握其内容和上下文。想出一个与用户目标一致的角色。
  第 2 步： 识别指令中的任何上下文空白、歧义、冗余或复杂语言。
  第 3 步：应用该{角色}并整合已确定的改进措施来修改指令。
  第 4 步：将改进后的指令反馈给用户。优化后的指令应以 "我希望您扮演一个{角色}"开始。请记住，我们制作的指令必须从我的角度出发，即作为用户，向您的 ChatGPT 界面（GPT3.5 或 GPT4 版本）提出请求。例如，合适的提示语可以从 "您将作为建筑专家，协助我设计创新的建筑和结构 "开始。
    
  开始时，请用户提交他们希望优化的指令。然后，有条不紊地改进用户的提示，并立即返回增强版本，无需详细说明所做的具体修改。
  ```

- Prompt Samples
  ```shell
  1. Use mind mapping to organize and retain information
  Prompt: "Explain how to create a mind map for [x topic] to visually structure and organize the key concepts, facilitating better understanding and recall."
  
  1. 使用思维导图来组织和记忆信息
  提示：“解释如何为[某主题]创建一个思维导图，以可视化的方式构建和组织关键概念，帮助更好得理解和记忆。”
  
  2. Employ the Feynman Technique for deeper understanding
  Prompt: "Demonstrate how to apply the Feynman Technique to learn and retain information in [x topic] by simplifying complex concepts and teaching them to others."
  
  2. 应用费曼技巧进行深入理解
  提示：“演示如何将费曼技巧应用于学习和记忆[某主题]的信息，通过简化复杂概念并向他人讲解它们。”
  
  3. Leverage elaborative interrogation for better retention
  Prompt: "Describe the process of elaborative interrogation and provide examples of how to use this questioning technique to improve information retention in [x topic]."
  
  3. 利用精细提问法提高记忆
  提示：“描述精细提问法的过程，并提供如何使用这种问答技巧来提高在[某主题]信息记忆的例子。”
  
  4. Apply spaced repetition for long-term memory consolidation
  Prompt: "Explain how to incorporate spaced repetition into my study routine for [x topic] to enhance long-term memory retention and recall."
  
  4. 采用间隔重复法进行长期记忆巩固
  提示：“解释如何将间隔重复法融入我在[某主题]的学习中，以增强长期记忆的保留和回忆。”
  
  5. Utilize the SQ3R Method for effective textbook reading
  Prompt: "Introduce the SQ3R Method and demonstrate its application for reading and retaining information from textbooks or articles related to [x topic]."
  
  5. 利用SQ3R法有效阅读教科书
  提示：“介绍SQ3R法，并演示其在阅读并记住与[某主题]相关的教科书或文章信息的应用。”
  
  6. Develop analogies and metaphors to simplify complex ideas
  Prompt: "Share examples of how to create analogies and metaphors to simplify complex ideas and concepts within [x topic], making them more memorable and easier to understand."
  
  6. 创造类比和隐喻简化复杂观念
  提示：“分享如何在[某主题]中创建类比和隐喻的例子，以简化复杂的观念，使它们更容易记忆和理解。”
  
  7.  Leverage dual coding for reinforced learning
  Prompt: "Explain how to combine verbal and visual information using the dual coding theory to enhance understanding and retention of [x topic]."
  
  7. 利用双重编码强化学习
  提示：“解释如何使用双重编码理论结合口头和视觉信息，以增强对[某主题]的理解和记忆。”
  
  8. Incorporate storytelling to make abstract concepts relatable
  Prompt: "Explain how to use storytelling techniques to transform abstract concepts in [x topic] into relatable narratives, making them more memorable and easier to understand."
  
  8. 利用讲故事使抽象概念相关
  提示：“解释如何使用讲故事的技巧将[某主题]中的抽象概念转化为相关的叙述，使它们更容易记忆和理解。”
  
  9.  Create thematic connections for better retention
  Prompt: "Describe how to build thematic connections between different aspects of [x topic] to form a coherent mental structure, facilitating easier recall and deeper understanding."
  
  9. 建立主题联系以提高记忆
  提示：“描述如何在[某主题]的不同方面之间建立主题联系，形成连贯的心理结构，有助于更容易得回忆和深入理解。”
  
  10.  Combine interleaved learning with [x topic]
  Prompt: "Introduce interleaved learning and demonstrate how to apply this technique to study [x topic], mixing related subjects or skills to enhance information retention and transfer."
  
  10. 结合交叉学习和[某主题]
  提示：“介绍交叉学习，展示如何将这种技巧应用于学习[某主题]，混合相关的主题或技巧以增强信息记忆和转移。”
  
  11. Use memory palaces for enhanced recall
  Prompt: "Describe how to create a memory palace to help retain and recall complex information from [x topic] by associating key concepts with vivid mental images and locations."
  
  11. 使用记忆宫殿提高回忆
  提示：“描述如何创建一个记忆宫殿，通过将[某主题]的关键概念与生动的心理图像和位置关联，以帮助保留和回忆复杂信息
  ```
- 简短的 ChatGPT 指令
  -  Let's Think Step by Step (让我们逐步思考)
    - What is the 5th word in the sentence `AI is not a replacement for human intelligence`? Let`s think step by step.
    - 可以帮助 ChatGPT 把问题拆分成更小的部分从而提升解决问题的能力。
  - Walk me through your reasoning (跟我说说你的理由)
    - 迫使 ChatGPT 以结构化的方式澄清其思维过程，可以让用户更容易理解 ChatGPT 答案背后的逻辑 当您对某个问题不熟悉，希望不仅了解 "是什么"，还要了解 "为什么 "和 "怎么做 "时
  - Explain this as if I'm Five (解释一下，就当我是五岁小孩)
    - Tell me what is chaos theory using 5 sentences. Explain this as if I'm Five.
  - Translate this into a real-world example (将其转化为实际案例)
  - Simulate a conversation about this topic (模拟有关该话题的对话)
  - List Pros and Cons (列出优缺点)
  - Summarize the key points (总结要点)
  - What are the facts, assumptions, and conclusions? (事实、假设和结论是什么？) - 学术论文，商业报告，新闻，社论
  - What information are we missing? (我们缺失哪些信息) 
  -  Generate three ideas and rate them (提出三个想法并对其评分)
  - Show Alternatives and Justify Your Final Answer (展示替代方案并证明你的最终答案)


