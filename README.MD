"#freerambo.github.io" 

Favourable response. 


Wanted to send you an update on VCEH progress as per latest discussions within the team. We still have till EOD tomorrow to send updated status to everyone, but wanted to start the discussion just with you.
 
XXX, XXX and others have evaluated current code base (still ongoing) to determine how much change is involved. We are having almost daily meetings with Trishell and Mitch as needed - the team also explicitly called out some baseline criteria which we have to adhere to, if we are expected to make an accelerated project delivery.
 
Thejanee's email below documents these criteria and we are getting product and architecture teams to agree formally prior to our start of development.
 
Even with this plan in place, the team unanimously feels we cannot give functional sign-off for Feb Release by Jan 28 (as per RM published timeline). This is because we shall not be able to meet DEV and QA quality gates that our team upholds within this timeframe.
 
Other than just the amount of new development and testing needed, one more key deterrent is that almost every backend method in VmeServices for this flow will need extensive and intrusive changes, as they are tightly coupled with how UI sends data to backend and is not really built for backend to backend API call. Needless to say, this is extremely risky and needs time for thorough regression, as we do not want to break SRC UI functionality.
 
 
So realistically we have 2 options:
 
1. Get an exception from all stakeholders to go for March Release (functional sign-off on Feb 25), along with rest of infra changes (if we cannot wait for regular April product Release).
 
2. Push out functional sign-off for this one feature by 2 weeks to Feb 11 (leaving 1 week for certification, pen and perf testing to complete) and make a very late entry into Feb Release. Obviously, this needs buy-in from all these teams + RM.
 
We can comfortably meet option 1 timelines without any compromises. Option 2 is a last ditch effort to make the cut for Feb Release and even if other supporting teams agree, it is going to stretch everyone to the limit in getting it right and leaves very little buffer to recover if everything does not go as per plan.
 
 
Wanted to take your opinion on option 1 vs 2 and I can start the discussions with stakeholders accordingly. Dropping this note to set the context tonight itself – can definitely discuss more in a call with you tomorrow if you need additional input.



> 上士闻道,勤而行之 中士闻道,若存若亡 下士闻道,大笑之,不笑不足以为道

> 有道无术，术尚可求也，有术无道，止于术



语文课上。鲁迅写下：哈哈。
语文老师：好！实在太好了！第一个“哈”字，表达了作者对当时黑暗社会的苦笑与无奈，第二个“哈”字，笔锋一转，作者意图传递一种积极乐观的情绪给读者，警惕国民，必须清醒，冲破旧制度的枷锁。而句末的句号，则运用得出神入化。句号表示结束，作者用简单一个句号，便充分强烈表达出要结束旧制度的想法。鲁迅写下：老师，你有问过我是怎么想的么？语文老师：棒！真的太棒了！作者用“老师”开头，说明其对文化的敬畏，也符合作者弃医从文的选择。而一句“你有问过我是怎么想的么”，一语双关。一方面是对社会的拷问，对社会压迫底层人民表达强烈的愤慨；另一方面，则是对麻木的国民的责问，责问人们为何从不敢说这句话。提醒国民要保持思考独立，保持质疑的精神，切勿人云亦云。也照应了开头，强调文化力量的可贵。这种找不到任何瑕疵的写作手法，值得大家学习。鲁迅写下：有病！语文老师：对！说得太对了！作者用铿锵有力的两个字，用近乎呐喊的语气，精辟深刻揭露了当时社会的问题所在：有病！社会病在腐朽落后，国民腐朽在麻木昏睡。这也为作者后来写《药》埋下了伏笔。前后呼应，滴水不漏，天衣无缝。鲁迅写下：呵呵。语文老师：绝！真是太绝了！作者用尖锐冰冷的笔调，精简又准确地表达了对当时那个吃人的社会以及阿Q们的控诉。所谓谈话止于呵呵，因此这二字也表明作者在必须改变社会现状和人们麻木思想的问题上，没有任何商量的余地，坚决而果断。好文！鲁迅写下：......语文老师：高！相当的高啊！面对这个病态的社会。千言万语，汇成一串省略号。看似什么都没讲，但又可以看作讲了无数话。并且，作者在表达自己态度的同时，又给与读者想象的空间。一千个人心中，有一千种对社会的看法。这也正是作者想表达的意思：人人独立思考，才能不麻木，才能救社会于危难......


