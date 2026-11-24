# ChowLine

## A meal pre-order and pickup system for campus cafeterias

**BU-209 Backend Engineering, Week 4**
Babcock University, Ilishan-Remo, Ogun State

### Group 8

| # | Name | Matric no. |
|---|---|---|
| 1 | Tella Uthman Tiwatope | 25/1708 |
| 2 | Oturomuo Kelvin Ogheneyole | 25/2567 |
| 3 | Tanimowo Obaloluwa David | 25/0174 |
| 4 | Agbeniga Anjolaoluwa | 25/0375 |
| 5 | Opanuga Ivan Oluwatomini | 25/0807 |
| 6 | Onele Chikeluba Michael | 25/0836 |

---

## A note before we start

We picked this problem because it is one we live with. All six of us eat on campus, and all six of us have stood in that queue at 12 o'clock wondering whether we would still make the next lecture.

Two things you should know while reading.

First, we have not finished collecting our data. We started the fieldwork but it is still running, so in the research section some things are findings and some are still what we expect to find. Where that is the case we say so. We would rather leave a table half empty than fill it with numbers we made up.

Second, we have deliberately not chosen a programming language, framework or database. The brief asked us to understand the problem and work out how to solve it before picking tools, so the solution section talks about what the system must do and what must never go wrong. It does not talk about code.

---

## 1. Product overview

ChowLine is a website where a student orders food from a campus cafeteria ahead of time and picks a time to collect it.

The student opens the site, sees what a cafeteria is cooking and how many portions are left, picks a meal, picks a ten minute pickup window, and pays. They get a short code on their phone. At that time they walk over, show the code, and collect the food without joining the queue.

On the other side, the cook sees a list of what has been ordered and for when, instead of guessing from the queue in front of them.

We are not building a delivery app. Nobody is bringing food to a hostel. The food is already two hundred metres away. The thing that costs students time is the queue and the fact that they cannot reserve a time.

The first version covers one campus, three to five cafeterias, students only, payment before pickup, a simple screen for the kitchen, and a pickup code.

---

## 2. The problem

### 2.1 What we see at lunch

Campus eating is squeezed into short windows, and the timetable decides those windows for you. A lecture ends at 12:00, several hundred students want food at the same moment, and most of them have about an hour before the next class.

The cafeteria cannot grow to meet that. There are a fixed number of serving points and a fixed number of staff, and the food is cooked in big batches. You cannot hire a cook for twenty minutes.

So three things clash at once. Everybody arrives together because the timetable pushes them together. The kitchen cannot get bigger for one hour. And the kitchen cannot see what is coming, because its only information is the queue already standing in front of it.

We assumed at first that the queue was caused by slow service. When we sat down to think about it properly, that did not hold up. A queue that long is not usually a sign that people are working slowly. It is a sign that more people arrived than the counter can physically serve in that hour.

### 2.2 The four things that actually go wrong

It would be easy to call this one problem. It is really four, and they need different answers.

**The waiting, and not knowing how long it will be.** The waiting is bad, but the uncertainty is worse. If a student knew the queue would take thirty minutes they could decide not to bother. Because they do not know, they either go early and waste time or stand there hoping. Either way something is lost.

**The kitchen cooking the wrong amount.** Sometimes the food finishes before the lunch period ends, and students who planned to eat there get nothing. Sometimes there is food left over and it is thrown away. Both happen because nobody knows how many people are coming.

**Pre-orders that cannot be trusted.** Some students already order ahead by calling or messaging a caterer they know, then sending a bank transfer and a screenshot on WhatsApp. The problem is that a screenshot proves nothing. Anyone can produce one, and during a rush nobody has time to open a banking app and confirm that ₦1,500 actually landed. So caterers either refuse pre-orders or end up arguing with students about money, and then everyone goes back to queuing.

**No record when something goes wrong.** Food not ready, wrong item, or the student was held back by a lecturer. Nothing is written down, so there is nothing to check afterwards and no refund process. The student loses money and the caterer loses trust.

### 2.3 Who this affects

| Who | What it costs them | How often |
|---|---|---|
| Students living on campus | Queue at breakfast, lunch and dinner | Every day |
| Students who commute | Less spare time, and no hostel kitchen to fall back on | Every day |
| Students with dietary needs | The queue is bad, and the option they can eat often finishes first | Every day |
| Cafeteria owners | Cannot plan how much to cook, and handle cash under pressure | Every day |
| Cafeteria staff | Taking orders verbally in a rush, making mistakes, counting cash | Every day |
| Student affairs and the catering office | Get complaints but have nothing to measure them with | Every week |

### 2.4 What people do about it now

People are already trying to solve this themselves. That is the most useful thing we found, because it means we do not have to teach anybody a new habit.

| What they do | Why they do it | Why it does not really work |
|---|---|---|
| Go 20 or 30 minutes early | To beat the crowd | The queue just moves earlier. Nobody actually saves time |
| Send a friend to queue | Time is worth trading | Wrong meal, or the friend eats it |
| Call or message the caterer | They already want to pre-order | No limit on orders, no record, no proof of payment |
| Pay by transfer and send a screenshot | They are willing to pay before eating | Screenshots can be faked and cannot be checked in a rush |
| Skip lunch and buy biscuits | Cheaper than losing the lecture | The real cost is health, not just time |
| Buy from vendors outside the gate | It avoids the queue | Costs more, takes time to get there, safety is unknown |
| Buy two meals in one visit | It saves a second queue | Food is cold by evening, and there is no flexibility |

The WhatsApp habit is the important one. It tells us the demand for pre-ordering already exists. The tool is just bad.

### 2.5 Why the obvious fixes do not work

We looked at what already exists before deciding to build anything.

| What exists | Why it does not solve this |
|---|---|
| Just queue | One person cannot fix a queue caused by hundreds arriving together |
| A WhatsApp group | No limit on how many people order, no payment check, no record once the chat scrolls |
| Google Forms | First come first served with no real limit, duplicates, no payment, no proof of collection |
| Food delivery apps | Built for riders and restaurants far away. A delivery fee for food you walk past |
| The cafeteria cash register | Records a sale at the counter. That fixes bookkeeping, not queuing |
| The university portal | Made for fees and registration, not for a twenty minute daily task |
| Just cook more | The kitchen cannot expand for one hour, and extra pots at peak cost more than they earn |

None of these gives the student a promise about time, and none of them gives the kitchen a forecast. That is the gap we are aiming at.

### 2.6 Problem statement

On our campus, thousands of students have to eat inside the same short windows each day, and the kitchens serving them cannot get bigger. A student has no way to reserve a meal for a particular time, and the kitchen has no way to know how many people are coming. So students queue for unpredictable amounts of time or skip meals altogether, kitchens cook the wrong quantity, and the pre-orders that do happen fail because nobody can verify the payment.

---

## 3. Research

### 3.1 What we wanted to find out

We wrote these down before starting so we would not just go looking for things that agreed with us.

1. How long do students actually wait, and how much does it vary?
2. What do students do instead, and what does that cost them?
3. Will students pay before they see the food?
4. What is the kitchen's real limit: staff, equipment, space, or the way food is cooked?
5. How does the kitchen decide how much to cook now, and would it trust a list of orders?
6. What happens today when a pre-order goes wrong?
7. Will a caterer accept orders that come through a system instead of from a person they know?

### 3.2 How we are collecting it

| Method | Who | How many | What it gives us |
|---|---|---|---|
| Watching and timing queues | 2 to 3 cafeterias | 6 sessions of 45 minutes | Real waiting times instead of guesses |
| Survey | Students on campus | 60 or more | Numbers on how common the problem is |
| Interviews | Students | 8 to 10 | Detail, stories, the words people actually use |
| Interviews | Cafeteria owners and managers | 3 to 4 | What the kitchen can really do |
| Interview | Student affairs or catering office | 1 or 2 | Rules, permissions, concerns |
| Reading | Public sources and similar products | | What already exists and what we can learn from it |

We are not only asking our own classmates. At least a third of the survey responses have to come from students we approach at the cafeteria itself, otherwise the sample is just one department talking to itself.

We also watch at different times and on different days, because Monday lunch and Saturday lunch are not the same thing.

### 3.3 What we have found so far

**Queue timings.** We are still running these. The table below is the sheet we are filling in, and the sessions marked with a dash have not been done yet.

| Session | Cafeteria | Day and time | Serving points open | In queue at start | Served in 45 min | Seconds per student | Longest wait seen |
|---|---|---|---|---|---|---|---|
| O-1 | | | | | | | |
| O-2 | | | | | | | |
| O-3 | | | | | | | |
| O-4 | | | | | | | |
| O-5 | | | | | | | |
| O-6 | | | | | | | |

**Working out what the kitchen can actually do.** Once we have the numbers, the calculation is simple:

- Students served per hour = serving points × (3600 ÷ seconds per student)
- Capacity for the lunch period = students per hour × length of the period in hours

To show how it works, take four serving points and thirty seconds per student. That is 480 students an hour. In a two hour lunch period that is 960 meals, which is the most that kitchen can produce. If 1,200 students want to eat in that period, the kitchen is being asked for a quarter more than it can do.

That missing quarter does not spread itself politely across the two hours. It piles up at the busiest moment, which is exactly when the student is standing there.

We are writing this down now because it cuts both ways. If we measure it and find the kitchen is only 80% full at peak, then the queue is not a capacity problem and this product is not needed. We would have to say so.

**Survey.** Still open. These are the numbers we are collecting.

| What we are measuring | Result | Responses |
|---|---|---|
| Total responses | | |
| Say they wait "too long" at least three times a week | | |
| Average wait they report | | |
| How that compares with what we timed ourselves | | |
| Have skipped a meal because of the queue in the last two weeks | | |
| Have sent someone else to queue for them | | |
| Have pre-ordered by phone, WhatsApp or transfer | | |
| Of those, how many had it go wrong | | |
| Willing to pay before collecting | | |
| Highest extra fee they would accept | | |
| Would use a pickup time instead of queuing | | |

**Interviews.** We are coding these by theme. A theme only goes in the table if at least three separate people mentioned it.

| What came up | How many mentioned it | What it means for us |
|---|---|---|
| Waiting is unpredictable, and not knowing is worse than the wait | | We should show an expected wait, not just a shorter one |
| Sending a friend is normal | | We need a rule on whether a pickup time can be given away |
| Fear of paying before seeing food | | Paying first may need to be introduced slowly |
| "The one I can eat finishes before I get there" | | We have to track each item, not just count orders |
| The rush comes from the timetable, not from choice | | Pickup times must fit lecture gaps, not round numbers |

From the cafeteria side we are asking how they decide how much to cook, what happens when they get it wrong in each direction, and what they would need to see before trusting a list of paid orders. One thing we already expect to hear is that they do not want an app, they want fewer arguments and less food in the bin.

### 3.4 What would make us stop

We agreed on this before starting, so we cannot pretend afterwards that we always intended to carry on.

- If the kitchen is under 80% full at peak at every cafeteria we watch, the queue is not a capacity problem and we should stop.
- If fewer than a quarter of students say they wait more than fifteen minutes at peak, the problem is not painful enough to build for.
- If almost nobody will pay before collecting, and no cafeteria will take payment on collection, the model does not work.
- If fewer than two of the four cafeterias we approach agree to try it, there is no supply side.
- If the payment charges are higher than any fee we could reasonably collect, it cannot pay for itself.

---

## 4. Business objectives

| # | What we want | How we will know |
|---|---|---|
| 1 | Cut the time students spend waiting at lunch | Average wait at pickup is five minutes or less |
| 2 | Give the kitchen a useful picture of demand before the rush | At least 70% of orders arrive before cooking starts |
| 3 | Replace the screenshot pre-order with something that can be checked | No payment disputes based on screenshots during the pilot |
| 4 | Make sure the service does not lose money on every order | The fee covers the cost of running it |
| 5 | Give the school something to measure complaints with | A weekly report the catering office can actually use |

We are not trying to change how the university bills students, replace the finance office, or negotiate with caterers on the school's behalf.

---

## 5. Target users

### Tolu, 200 level, lives on campus

She eats on campus three times a day because going off campus costs transport money. She has a 45 minute gap between two lectures, and that gap is exactly when everybody else eats.

She has skipped lunch twice this month and does not think it is a big deal. Her phone is a mid-range Android with a data bundle she buys weekly, and she assumes any website will be slow and eat her data. She will not read instructions, and she will close anything that takes longer than a minute and a half.

What she wants is simple. She wants to know her food is hers and she wants to know what time to walk over.

### Mama Ngozi, cafeteria owner

She runs one cafeteria with five staff and three serving points. She decides how much to cook from experience and from how the queue looks at 11:40.

She has been shown a transfer screenshot before and never received the money. She does not want an app. She wants fewer arguments, less cash to count, and less food going in the bin.

What she wants is to know by 11:45 that she is cooking 180 plates, instead of guessing.

### Mr Ade, student affairs

He gets complaints about food but cannot measure them, and he is the one responsible if something goes wrong with a vendor.

What he wants is a report he can point at instead of repeating what students told him.

### Who we are not building for

Students who want food delivered to their hostel room. That is a different product with a different cost, and the student is walking past the cafeteria anyway. Off campus restaurants are also out of scope. Staff and lecturers are welcome later, but they eat at different times and mixing them in would make our numbers confusing in the first version.

---

## 6. Stakeholders

| Who | What they care about | How much say they have | How we work with them |
|---|---|---|---|
| Students | Time, price, whether it actually works | High, they decide if it gets used | Survey, recruit for the pilot, feedback in the app |
| Cafeteria owners | More sales, less waste, staying in control | High, they can refuse to join | Interviews, a pilot agreement, weekly review |
| Cafeteria staff | Workload, clear instructions | Medium | Watch how they work, design around it |
| Student affairs and catering office | Safety, complaints, oversight | High, they give permission | Formal briefing, share reports |
| University IT | Internet and security | Medium | Ask about campus network limits early |
| Payment provider | Verification, settlement, disputes | Medium | Read their rules before designing anything |
| Group 8 and our lecturer | Delivering what we promised | High | Weekly check against this document |

---

## 7. User stories

**Finding food**

- As a student I want to see which cafeterias are open right now, so I do not walk to a closed one.
- As a student I want to see the meal, the price and what is in it, so I can decide without asking anybody.
- As a student I want to see how many portions are left, so I do not order something that is finished.
- As a student with a dietary need I want to filter the menu, so I do not find out at the counter that the only thing I can eat is gone.

**Choosing a time**

- As a student I want to pick a pickup time and see if it is free, so I can fit it between lectures.
- As a student I want to be told clearly when a time is full, so I am not promised something the kitchen cannot do.
- As a student I want my meal held while I pay, so it is not taken from me in the middle of paying.
- As a student I want to cancel before the deadline and know whether I get my money back, so a change of plan is not a loss.

**Paying**

- As a student I want to pay inside the site, so I do not have to send screenshots to anybody.
- As a student I want a clear message that my payment worked, so I know whether to start walking.
- As a student, if my payment fails I want my time slot handled fairly, so a bad network does not cost me lunch.

**Collecting**

- As a student I want a code on my phone that works only once, so nobody else can use it.
- As a student I want that code to work even when the network is down at the counter, so I am not punished for something I cannot control.
- As a student I want to know my food is ready before I walk over, so I do not wait at the counter instead.

**For the kitchen**

- As a cafeteria owner I want one list of orders by time, so I can cook in batches.
- As a cafeteria owner I want to accept or reject an order, so I stay in control.
- As a cafeteria owner I want to mark an item as finished, so the menu matches what is actually left.
- As a cafeteria owner I want to set how many portions each time window can take, so I am never promised more than I can cook.
- As a cafeteria owner I want to check a pickup code in one tap, so the counter does not slow down.
- As a cafeteria owner I want a daily statement, so I can match money received with meals served.

**For the administration**

- As an administrator I want to add and suspend cafeterias, so only approved kitchens sell food.
- As an administrator I want a weekly report, so complaints become numbers.
- As an administrator I want a full record of every order, so disputes can be settled with facts.

**Trust**

- As a student, if the kitchen cannot fulfil my order, I want my money back without begging anybody.
- As a cafeteria owner, if a student does not show up, I want a rule that is applied the same way every time.
- As a student I want to know my data is not being sold, so using this does not cost me privacy.

---

## 8. How we plan to solve it

### 8.1 The idea

Three things.

A **serving period**. Lunch runs from 12:00 to 2:00, and that is a real window with a real end.

A **pickup window**. Ten minutes inside that period, with a limit on how many meals it can take.

An **order**. One student, one meal, one time, paid for before they walk over.

Everything else in the product comes out of those three.

### 8.2 Why pickup windows instead of a queue number

Our first idea was a queue number app. You take a number, you watch it, you go when it is close. We dropped it after thinking about what it actually changes.

A number tells you where you are in a line. That is only useful if the line moves at a steady speed, and it does not. What actually limits the kitchen is when a batch of food is ready, not how many people are waiting.

Worse, a number does nothing about capacity. If 500 people take a number for a kitchen that can serve 300, the app has just made 200 people wait in a different place, and if they have paid, it has made things worse than the queue we have now.

A time window with a limit is more honest. It matches what the kitchen can actually do, and it lets us refuse a student before they pay instead of disappointing them at the counter.

### 8.3 What a student does

1. Opens the site and sees which cafeterias are open.
2. Picks a meal and sees how many are left.
3. Picks a pickup time. Full times cannot be selected, and nearby free ones are suggested.
4. The meal is held for a few minutes while they pay.
5. Pays by card or transfer.
6. Gets a code and the name of the counter.
7. Walks over at that time, shows the code, collects the food.

If they abandon payment, the hold expires by itself and the meal goes back on the menu for somebody else.

### 8.4 What the cafeteria does

1. Sets the menu for the day and how many portions each window can take.
2. Sees a count of what to cook, by item and by time, before the rush.
3. Marks an item as finished when it runs out, and it disappears from the menu.
4. Accepts or rejects orders.
5. Checks pickup codes at the counter in one tap.
6. Gets a statement at the end of the day.

### 8.5 The parts we know will be difficult

**Making the limit real.** The most important thing this system has to do is stop more orders than the kitchen can handle. That sounds easy until two students try to take the last plate in the same second. If we are careless, both can be told they succeeded and both can pay, and then one of them walks over to nothing.

The fix is that the check and the taking have to be one action. A student is only let through if the plate is still there at that exact moment. We will test it on purpose by sending fifty orders at once at a meal with one portion left, and exactly one should succeed. We would rather find that bug ourselves than have it happen in the cafeteria.

**Making payment confirmation reliable.** Payment providers sometimes send the same confirmation twice, or send it late, or not at all when the network is bad. If we treat a repeat as a new payment we can confirm an order twice or refund it twice. So the system has to be built so the same confirmation can arrive any number of times and only count once, and so that any payment we are unsure about gets checked with the provider instead of guessed at.

**Deciding what a limit means.** Is it 40 orders per window, or 40 plates of jollof and 20 of rice? Those are different things, and only the owner can tell us which one matches how they cook. We are treating it as an open question rather than assuming.

**What happens when somebody does not show up.** The food has already been cooked. Somebody has to carry that loss, and the rule has to be written down and applied the same way every time or it will cause arguments.

### 8.6 Ideas we considered and dropped

| Idea | Why we dropped it |
|---|---|
| A queue number app | It does not add capacity. It just moves the crowd to one moment at the counter |
| Order ahead and "we will call you" | The waiting comes back, just in a different place |
| Payment on collection only | Easier for the student, but the kitchen loses the forecast and the student loses the guarantee. We are keeping this as a backup if people refuse to pay first |
| A phone app first | Costs more and is harder to get people to install. A website opens from a link and we can test it in a day |
| Food delivery | Different product, and the student is walking past the cafeteria anyway |
| Charging cafeterias a monthly fee | Cannot be tested in a short pilot, and it does not tell us whether students actually want this |

### 8.7 What we are not deciding yet

We have not chosen a language, a framework, a database or a hosting provider, and we are not going to in this document.

Before we pick anything we need to know four things.

How busy is it really at peak? That tells us whether an ordinary setup can handle it.

What does the team already know how to build and run? Using something we can operate well beats using something fashionable.

How reliable is the network at the counter? That decides whether the pickup code has to work offline.

What can we afford to run each month? It has to cost less than we make per order.

When we do choose, the test is whether we can build this without breaking the "never sell more than the kitchen can cook" rule, whether we can run it properly, and whether we can afford it. The language is the least important part of that.

---

## 9. Functional requirements

Priority: **M** means it has to be in the first version, **S** means we want it soon after, **C** means later.

### Finding food

| # | Requirement | Priority |
|---|---|---|
| F1 | Show cafeterias that are open now, with their serving period | M |
| F2 | Show each meal with name, price, description and dietary tags | M |
| F3 | Show how many portions are left for each meal in each time window | M |
| F4 | Let students filter meals by dietary need | S |
| F5 | Show photos of meals | C |

### Choosing a time and ordering

| # | Requirement | Priority |
|---|---|---|
| F6 | Let a student pick a time window, and make full ones clearly unavailable | M |
| F7 | Hold the meal for a few minutes while the student pays | M |
| F8 | Release the hold automatically if payment does not happen | M |
| F9 | Follow the order steps in order and refuse anything out of order | M |
| F10 | Let a student cancel before the deadline and get the agreed refund | M |
| F11 | Reorder a previous meal in one tap | C |

### Paying

| # | Requirement | Priority |
|---|---|---|
| F12 | Pay by card or transfer inside the site | M |
| F13 | Confirm a payment only once, even if the provider sends the message more than once | M |
| F14 | Check with the provider when a payment result is unclear | M |
| F15 | Keep a permanent record of every payment and refund | M |
| F16 | Refund automatically when an order cannot be fulfilled | M |
| F17 | Store and calculate money in whole kobo, never decimals | M |
| F18 | Produce a daily statement for each cafeteria | S |

### Collecting

| # | Requirement | Priority |
|---|---|---|
| F19 | Give the student a code that works only once | M |
| F20 | Let staff check a code in one tap, and refuse a second use | M |
| F21 | Let the code work when the network is down, and catch up later | S |
| F22 | Tell the student when the food is ready | S |
| F23 | Mark orders as not collected after the grace period | M |

### For the cafeteria

| # | Requirement | Priority |
|---|---|---|
| F24 | Show orders by time window and by meal | M |
| F25 | Accept or reject an order, with a reason | M |
| F26 | Mark an item as finished, with immediate effect | M |
| F27 | Set and reduce the limit for each time window | M |
| F28 | Close the serving period | S |
| F29 | Show today's orders and money | S |

### For the administration

| # | Requirement | Priority |
|---|---|---|
| F30 | Add, suspend and remove cafeterias | M |
| F31 | Show the full history of any order | M |
| F32 | Record every change with who did it and when | M |
| F33 | Issue a manual refund with a written reason | M |
| F34 | Weekly report | S |
| F35 | Export orders and payments | C |

### Accounts

| # | Requirement | Priority |
|---|---|---|
| F36 | Students sign in with their school email or a verified student ID | M |
| F37 | Cafeteria staff can only see their own cafeteria | M |
| F38 | Check permissions on the server, not just hide buttons in the screen | M |
| F39 | Limit repeated failed logins and record them | M |

---

## 10. Non-functional requirements

These are the ones we will be judged on technically, so we have given each a number instead of a vague word like "fast" or "secure".

| # | Requirement | Target | How we check |
|---|---|---|---|
| Q1 | Browsing the menu is quick | Under half a second for most people | Load test |
| Q2 | Placing an order is quick | Under one second for most people | Load test |
| Q3 | Never sell more than the kitchen can cook | Zero oversells, even when many people order the last portion at once | Send many orders at once at a meal with one left |
| Q4 | Orders are always in a sensible state | No order stuck in a state it should not be in | Work through the order steps thoroughly |
| Q5 | A payment is never counted twice | Zero duplicates | Send the same confirmation repeatedly |
| Q6 | No order left in limbo after a payment problem | Every unclear payment resolved within 30 minutes | Switch the provider off mid-payment |
| Q7 | Available during meal times | Working 99.5% of the time between 7am and 8pm | Monitoring |
| Q8 | Recover from a failure | Lose no more than five minutes of data, back within an hour | Practise a restore |
| Q9 | Money is always exact | No rounding errors anywhere | Test with large orders |
| Q10 | Any order can be explained from its records | History rebuildable from the records alone | Check 20 random orders by hand |
| Q11 | Nothing secret in the code | No passwords or keys in the shared code | Scan before every upload |
| Q12 | One cafeteria cannot see another's data | Zero leaks | Try to access another account's data |
| Q13 | A pickup code cannot be guessed or reused | No successful reuse | Security test |
| Q14 | A student can finish in under 90 seconds | 4 out of 5 first-time users | Watch five people try it |
| Q15 | Usable by someone with poor eyesight | Works with a screen reader and larger text | Manual check |
| Q16 | Loads on a slow connection | Under three seconds on a slow phone | Throttled test |
| Q17 | Cost per order stays small | Under 15% of what we make per order | Monthly cost check |
| Q18 | Adding a cafeteria needs no coding | A new one can be added from the admin screen | Try it ourselves |

Q3 is the one we care about most. Any group can build a page that takes an order. Making it impossible to sell the same plate twice when fifty people click at the same moment is the actual engineering in this project, and we plan to show that test when we present.

---

## 11. Scope and priorities

### What is in the first version

- One campus, one currency, one payment provider.
- Three to five cafeterias, added by us by hand.
- A website that works on a phone. No app to download.
- A menu with limits per meal per time window.
- Hold, pay, confirm, pickup code, collect.
- A screen for the kitchen: order list, accept or reject, mark finished, set limits.
- Automatic refunds when an order cannot be fulfilled.
- A basic admin area: approve cafeterias, look up an order, refund, see the history.
- A permanent record of every order change and every payment.
- The test that proves we never oversell.

### What comes after the pilot

- "Your food is ready" notifications.
- Pickup codes that work with no network.
- Weekly reports for the school.
- Daily statements for cafeterias.
- Order history and one tap reorder.
- An estimated wait for students who decide to queue anyway.

### What comes later, if it works

- Linking to a prepaid meal plan or card.
- More than one campus.
- Proper allergy management.
- Delivery to hostels.

### What we are not doing at all

- Riders and delivery.
- Restaurants off campus.
- Ratings, reviews and social features.
- Anything where we hold card details.
- Anything that changes how the school charges students.

---

## 12. Acceptance criteria

These are the checks we will run before we say a feature is done.

**Overselling.** A time window has 40 meals and 40 orders. A student tries to choose it. The window shows as full, they are stopped before paying, and nothing is held.

**Two people at once.** A window has one portion left. Fifty orders arrive at the same moment. Exactly one succeeds, the other 49 are told it is finished, and the count shows zero. We repeat this a hundred times and expect the same result every time.

**Abandoned payment.** A student starts an order and closes the page. After a few minutes the order is cancelled, the meal returns to the menu, and another student can take it without us doing anything.

**Repeat payment message.** We take a payment confirmation we have already handled and send it again twice. Nothing changes, the student is not charged twice, and they are not sent a second confirmation.

**Unclear payment.** We never receive the confirmation for a payment. After some time the system checks with the provider and either confirms the order or refunds it. Nobody is left waiting.

**Using a code twice.** An order is ready and the code is used. The order shows as collected. The same code is shown again and is refused and recorded.

**Automatic refund.** A cafeteria rejects a paid order. The refund starts on its own, the student is told, and the record shows both the payment and the refund.

**Cafeteria privacy.** A staff account for Cafeteria A tries to open Cafeteria B's orders or money. It is refused and the attempt is recorded.

**Item finishing.** The owner marks an item as finished. A student opens the menu and the item is gone within a few seconds. Any order already paid for is handled by refund.

**Money is exact.** For any order, refund or statement, every amount is a whole number of kobo and adding the records up gives exactly what the student paid.

**A complete record.** For any finished order we can see the whole story: when it was held, the payment messages, what the cafeteria did, and when it was collected, with who did it and when.

**Adding a cafeteria.** A new approved cafeteria with its menu and limits can go live from the admin screen without us changing any code.

**Easy enough to use.** A student who has never seen it places an order in under 90 seconds without help, for at least four out of five people we test.

---

## 13. Assumptions and open questions

### Things we are assuming and still have to check

| # | We are assuming | How we will check |
|---|---|---|
| A1 | The kitchen is being asked for more than it can produce at peak | The calculation in section 3.3 |
| A2 | Serving one student takes about 25 to 40 seconds | Our own timing |
| A3 | Students have a smartphone with data and will use a website | Survey |
| A4 | At least one cafeteria will try it without being paid to | Interviews |
| A5 | Ten minute pickup windows are useful and practical | Owner interviews and our own test |
| A6 | Most students can pay by card or transfer when they order | Survey |
| A7 | The kitchen can cook to a list without the food suffering | Owner interviews and the pilot |
| A8 | Students will accept an extra fee of about ₦50 to ₦150 | Survey |

### Things we do not know yet

| # | Question | Why it matters | Who is finding out | Needed by |
|---|---|---|---|---|
| 1 | Is food paid per meal, or through a prepaid card or meal plan? | It changes how payment and payouts work completely | Research lead | Before we design payment |
| 2 | While the money is with us, whose is it? | Affects whether we are even allowed to hold it | Group and admin | Before the pilot |
| 3 | Is a limit a number of orders, or a number of plates of each meal? | Decides how we store limits | Owner interviews | Before we design the menu |
| 4 | How does an owner think about their limit in real life? | The number has to mean something to the person setting it | Owner interviews | Before we build the kitchen screen |
| 5 | Can a student give their pickup time to a friend? | Affects identity and fraud rules | Student interviews and school policy | Before the first version |
| 6 | Will owners share how much food they waste? | We need it to show value to them | Owner interviews | Before section 3.3 is finished |
| 7 | How long is the refund window, and who loses money on a no-show? | Fairness, and whether we can afford it | Owners and admin | Before the pilot agreement |
| 8 | Does the school need to approve us taking payments on campus? | It could stop the project | Admin interview | Immediately |
| 9 | How far in advance can somebody order? | Affects how long we hold orders and how the kitchen plans | Owner interviews | Before the first version |
| 10 | What happens if a student loses their phone that day? | We need a process even if it is not a feature | Group 8 | Before the pilot |

---

## 14. Risks

| # | Risk | How likely | How bad | What we do about it | How we would notice |
|---|---|---|---|---|---|
| 1 | Students will not pay before seeing food | Medium | High | Ask them properly before building, and be ready to allow payment on collection | Fewer than half of started orders get paid |
| 2 | Cafeterias will not join | Medium | High | Let them accept or reject orders, and start with one friendly owner | Fewer than two of four agree |
| 3 | A bug sells the same plate twice | Low | Very bad | Test it deliberately before launch, and keep a manual cap as a backup | Any report of it |
| 4 | Payment problems leave students waiting | Medium | High | Check unclear payments with the provider automatically | Orders paid but not confirmed for over 15 minutes |
| 5 | The network fails at the counter | High | Medium | Let codes work offline and catch up later | Codes failing to check |
| 6 | Food cooked for people who do not come | High | Medium | A written no-show rule, a reminder, and a limit the owner sets | More than 10% not collected |
| 7 | The fee does not cover the payment charges | Medium | High | Check real meal prices early, and charge the fee to the student | Losing money on every order |
| 8 | The school is uncomfortable with payments on our site | Low | High | Brief them early, and never hold card details | Delay in getting permission |
| 9 | The timetable changes and the rush moves | Medium | Medium | Owners set their own limits and can change them | A sudden change in how busy it is |
| 10 | We try to build too much | High | Medium | Section 11 is our agreement. Changes need a written decision | New ideas that are not in this document |
| 11 | Demand changes during exams or holidays | High | Medium | Set limits per day, not per week, and watch two different weeks | Big differences between weeks |

---

## 15. How we will measure success

| For whom | What we measure | Target after stage 3 |
|---|---|---|
| Student | Average wait at pickup | 5 minutes or less |
| Student | Orders actually collected | 85% or more |
| Student | People who order again within two weeks | 40% or more |
| Cafeteria | Orders placed before cooking starts | 70% or more |
| Cafeteria | Orders not collected | 10% or less |
| Cafeteria | Owners who want to continue | 2 of 3 or more |
| System | Oversells | Zero |
| System | Orders stuck waiting for confirmation | Zero |
| System | Payments counted twice | Zero |
| Business | Money made per order after costs | Above zero |
| Business | Disputes settled with records | 100% |

---

## 16. The pilot

We will start with one cafeteria, lunch only, for two weeks. We will invite students who filled our survey, so we already know roughly how long they used to wait. Each day we will also time students who queue normally, so we have something to compare against. Without that comparison, any improvement we report is just a story we are telling.

Every day we record orders placed, orders collected, people who did not come, the average wait at the counter (with a stopwatch, not by asking), any oversell, payment failures, refunds, and one sentence from the owner about how it went.

We would stop early if there is an oversell, if a student loses money we cannot explain, if the owner pulls out, or if everybody books the same popular time.

That last one matters more than it sounds. If we offer pickup times and everyone picks 12:30, we have not removed the queue, we have moved it. So the system has to spread people out, either by limiting each window or by making the earlier ones more attractive. We expect to learn which by watching the first week.

| Stage | How long | What has to be true to move on |
|---|---|---|
| Test ourselves | 1 week | We place 50 orders on a real menu and nothing breaks |
| One cafeteria | 2 weeks | At least 60 orders a week, no oversells, average pickup wait 5 minutes or less |
| Three cafeterias | 3 weeks | At least 250 orders a week, no unresolved disputes, the owner wants to continue |
| Review | 1 week | Decide whether to continue, change direction, or stop |

---

## Appendix A: Student survey

Target is 60 or more responses. It takes about three minutes. We are sharing it as a QR code at the cafeteria, not only in class groups.

**First question**

1. Do you eat food made on campus at least twice a week? (Yes / No. If no, stop here)

**Your experience, thinking about the last seven days**

2. How many times did you eat from a campus cafeteria in the last seven days? (0 / 1 to 3 / 4 to 6 / 7 to 10 / more than 10)
3. Think about your most recent visit at a busy time. How long did you wait before you were served? (under 5 / 5 to 10 / 10 to 20 / 20 to 30 / over 30 minutes / I left without eating)
4. Was that what you expected before you joined the queue? (shorter / about the same / longer / much longer)
5. In the last two weeks, have you skipped a meal because the queue was too long? (Yes / No)
6. In the last two weeks, has someone queued for you, or have you queued for someone? (Someone queued for me / I queued for someone / Both / No)
7. In the last two weeks, has the meal you wanted finished by the time you got to the counter? (Yes / No)

**What you do now**

8. How do you usually deal with the lunch queue? (tick all that apply) Go early / send someone / eat outside campus / skip the meal / buy snacks instead / order ahead by phone or WhatsApp / other
9. Have you ever ordered food in advance from a campus vendor by phone, WhatsApp or bank transfer? (Yes / No)
10. If yes, did anything go wrong? (tick all that apply) Food not ready / wrong item / they said they did not receive my payment / the food finished / no problem / other
11. If yes, how did you show that you had paid? (Bank transfer screenshot / paid on collection / they know me / other)

**Would you use it**

12. If you could reserve a meal and a ten minute pickup time in advance, how likely would you be to use it? (1 = never, 5 = every time)
13. Would you be willing to pay before collecting to be sure of your time? (Yes / No / It depends, please explain)
14. On a ₦1,500 meal, what is the most extra you would pay as a convenience fee? (₦0 / up to ₦50 / ₦50 to ₦100 / ₦100 to ₦200 / more)
15. What is the one thing that would stop you from using this? (your own words)
16. What time is your lunch break, and how long is it? (your own words)

**About you**

17. Level (100 / 200 / 300 / 400 / 500 / postgraduate)
18. Do you live on campus? (Yes / No)
19. Any dietary need we should know about? (optional)

Two notes on why the questions are worded this way. Questions 3 and 4 sit together on purpose: someone who waits 25 minutes and expected 25 has a different problem from someone who waited 25 expecting 5. Question 11 is the evidence for our payment problem. Question 13 is the real test of whether people will pay first, so it must not be softened into "would you like".

---

## Appendix B: Interview questions

### Students, about 30 to 35 minutes

We open with: "I'm not selling anything and there are no right answers. I'm trying to understand how lunch actually works for you. Can I record this? It stays anonymous."

1. Take me through your last weekday lunch, from the moment your lecture ended.
2. How did you decide where to eat?
3. How did you know how long the queue would be before you joined it?
4. What did you do while waiting?
5. Has waiting ever cost you something other than time, like a lecture, a meeting, or someone's patience?
6. Tell me about a time you did not get the food you wanted. What happened next?
7. Have you ever arranged food in advance? How, and what happened?
8. If a friend asked you to pre-order for them, would you? Why?
9. What would have to be true for you to pay before you see the food?
10. What would make you distrust something like this?

We let them talk through question 1 without interrupting, because that answer usually contains more than the rest combined. And if somebody says "about twenty minutes" we ask whether that is a guess or whether they checked their phone.

### Cafeteria owners, about 40 to 45 minutes

1. Take me through a normal lunch, hour by hour, from preparation to closing.
2. How do you decide how much to cook? What happens when you cook too much, and when you cook too little?
3. What is the real limit at peak: staff, equipment, space, or serving? Which one would you fix first?
4. If I told you 180 people would collect between 12:30 and 12:45, would that be easier or harder than the same 180 arriving at random? Why?
5. What do you do today when somebody orders in advance? How do you know they paid?
6. Tell me about the last time a pre-order went wrong.
7. What would you need to see before you trusted a list of paid orders?
8. If a student does not come to collect, who should lose the money? Why?
9. Who decides the menu each day, and how far in advance?
10. What would make you refuse to use something like this?

We listen for the words they use for quantity, whether that is plates, pots, portions or customers. Those words tell us what a "limit" should actually mean.

### Administration, about 30 minutes

1. How do complaints about food reach you, and what do you do with them?
2. What information do you wish you had about campus food?
3. What worries you about students paying for food through a website?
4. What would need to be true for the school to allow a pilot?
5. What are your food safety and vendor requirements?

---

## Appendix C: Queue timing sheet

One sheet per session. We use a phone stopwatch, not estimates.

| Item | What we recorded |
|---|---|
| Date and day of the week | |
| Cafeteria | |
| Which meal period | |
| How many serving points were open | |
| How many staff at the counter | |
| People in the queue when we started | |
| People in the queue at 15, 30 and 45 minutes | |
| How many were served during the session | |
| Average time per student (time 15 in a row, then divide) | |
| Longest wait (follow one person from joining to being served) | |
| Items that finished, and at what time | |
| How many people left without being served | |
| Prices we saw | |
| Anything unusual, like weather or a lecture that ended at 12:00 | |

Two rules for this. Count actual people, not a guess at people, and if somebody is holding a place for three friends, count the friends too and write it down. Always record what time a lecture block ended, because the timetable is what creates the queue in the first place, and a timing sheet without it cannot be compared with another one.

---

## Appendix D: Words we use

| Word | What we mean |
|---|---|
| Serving period | The time a cafeteria is open for a meal, for example lunch |
| Pickup window | A short time inside that period when a student collects |
| Hold | Keeping a meal back for a few minutes while the student pays |
| Limit | The most portions a cafeteria agrees to serve in a time window |
| Oversell | Accepting more orders than the cafeteria can actually serve |
| No-show | A paid order nobody came to collect |
| Payout | Sending the cafeteria the money for the meals it served |
| Cut-off | The time after which orders or changes are no longer accepted |

---

## Appendix E: Sources

Each of these has to be checked and cited properly with a date before we submit. Nothing here is quoted from memory.

**Our own research**

- Queue timing sheets, sessions O-1 to O-6
- Survey responses, saved with the date they came in
- Interview notes, anonymised and kept separate from this document
- A list of every claim in section 3 and where it came from

**Payment provider**

- Their developer documentation on how payment confirmations are sent and what happens when they fail
- Their price list, including both the percentage and the fixed charge per transaction, which we need for the fee calculation

**Law and regulation**

- Nigeria Data Protection Act 2023, on collecting only what is needed, keeping it no longer than necessary, and people's rights over their data
- The rules on who is allowed to hold customers' money, which is question 2 in section 13

**Products we looked at for ideas**

- University meal ordering systems, for how they limit portions
- Order-ahead features in restaurant apps, for how they treat pickup times
- Appointment booking systems, like hospital appointments, for cancellations and people who do not turn up

---

## Appendix F: Changes to this document

| Version | Date | What changed | Who |
|---|---|---|---|
| 0.1 | | Chose the problem, started reading | Group 8 |
| 0.2 | | Fieldwork done, research section filled in | Group 8 |
| 0.3 | | Solution and requirements written | Group 8 |
| 1.0 | | Reviewed, first version agreed | Group 8 |

---

**GROUP 8**
