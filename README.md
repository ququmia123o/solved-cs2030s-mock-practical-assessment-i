Download Link: https://assignmentchef.com/product/solved-cs2030s-mock-practical-assessment-i
<br>
<h4></h4>

<b>Topic Coverage</b>

<ul>

 <li>Classes, Methods, Attributes and Methods</li>

 <li>Abstraction, Encapsulation, Inheritance and Polymorphism</li>

 <li>Collections</li>

 <li>Abstract Classes</li>

 <li>SOLID principles</li>

 <li>Generics</li>

</ul>

Please download the files needed for the exam <a href="https://drive.google.com/file/d/10z1bc-LejYHM5wPX9DSRVdzem8Aev_M5/view?usp=sharing">here</a>.

<h3>Problem Description</h3>

We are in the midst of a global crisis. SARS-CoV-2 (SARS CoronaVirus 2), the virus that causes COVID-19, has infected millions and killed thousands of people worldwide.

In Singapore, we have implemented two systems, SafeEntry and TraceTogether. Right now, you are a junior developer in the team that is developing the <strong>D</strong>isease <strong>O</strong>utbreak <strong>R</strong>esponse <strong>M</strong>anagement <strong>S</strong>ystem (<strong>DORMS</strong>), that brings both of these systems together. In addition, DORMS will automatically issue Stay-Home Notices (similar to a quarantine order).

The actual system needs a lot of time to develop. You’re in charge of completing a simulation of DORMS, which simulates how DORMS will operate when it has been implemented, and keeps track of statistics and the efficacy of the policies enacted.

The simulation environment of DORMS is extremely complex. As such, read the following explanations carefully.

<h3>The simulation</h3>

<h4>Viruses</h4>

In the simulation, there are three types of viruses. All viruses have some probability of mutating, and spread by creating another virus of the same type, or another virus of a different type (mutation). Here is how each virus spreads:

<ul>

 <li>AlphaCoronavirus: Every time it spreads, it has some probability of mutating into SARS-CoV-2. In the event that it doesn’t mutate, the virus will simply create another AlphaCoronavirus, but the probability of that new virus mutating is reduced by 10% (given by <tt>SimulationParameters#VIRUS_MUTATION_PROBABILITY_REDUCTION</tt>).</li>

 <li>SARS-CoV-2: This is the target virus of the simulation, and this causes COVID-19. Every time it spreads, it has some probability of mutating into a BetaCoronavirus. In the event that it doesn’t mutate, the probability of that new virus mutating is also reduced by 10%.</li>

 <li>BetaCoronavirus: This virus has no probability of mutating. As such, every time it spreads, it simply creates another BetaCoronavirus.</li>

</ul>




<h4>People</h4>

Of course, people are the key of this simulation, who are the primary vectors of the viruses above. People can transmit (give out) viruses they have, and can be infected with (take in) viruses from a contact. This simulation concerns itself with two types of people:

<ul>

 <li><tt>Person</tt>: Represents the average person like you and me. When transmitting viruses, the person will transmit all the viruses he/she is infected with.</li>

 <li>MaskedPerson: This person is wearing a mask. In the simulation, masks are 60% effective (given by <tt>SimulationParameters#MASK_EFFECTIVENESS</tt>). This means that there is a 60% chance that at any given contact, no viruses will be transmitted. Likewise, there is a 60% chance that at any given contact, this person will not be infected with any virus.</li>

</ul>




<h4>DORMS</h4>

As a junior developer, you are not required to implement the full solution yourself. The higher level classes have already been implemented for you. All you need to do is to implement the concrete classes that represent different entities of the simulation. However, for clarity, the following explains how DORMS works and provides some specifications for you as you complete the implementation.

DORMS is essentially a combination of two existing solutions:

<ul>

 <li>SafeEntry: This is a system that allows users to check in and out of different locations. This allows us to keep track of all the contacts made in a location, to prevent and control the transmission of diseases and identify disease clusters. With SafeEntry, it is assumed that a person entering a location makes contact with everyone in that location, and diseases are spread via each contact.</li>

 <li>TraceTogether: This is a programme to enhance Singapore’s contact tracing efforts. The TraceTogether app is a mobile application that uses bluetooth to detect other nearby TraceTogether-enabled devices. On device detection, it is assumed that contact is made and diseases are spread via that contact.</li>

</ul>

Given these two systems, DORMS is a single platform that interacts with both of these systems for contact tracing. It allows users to check in to a location (via <tt>checkIn</tt>), check out of a location (via <tt>checkOut</tt>), keeps track of other contacts for TraceTogether (via <tt>contact</tt>). It also notes any person who presents with symptoms of respiratory illnesses (via <tt>presentSymptoms</tt>). DORMS will conduct a serological test on anyone who presents themselves with these symptoms, and take the necessary action if the person tests positive for the target virus (SARS-CoV-2). The following describes the action that needs to be taken by DORMS on a positive SARS-CoV-2 test:

<ol>

 <li>The person who tested positive will be given a 28-day Stay-Home-Notice and will not be allowed to leave their home during that period.</li>

 <li>All recent (14 days, or <tt>SimulationParameters#TRACING_PERIOD</tt>) contacts made by this person will be served a 14-day (or <tt>SimulationParameters#SHN_DURATION</tt>) Stay-Home-Notice as well.</li>

</ol>

However, the current implementation of DORMS does not support it, but thanks to SOLID principles, the developers of DORMS have made it open for extension, and it will be simple to implement this behaviour.




<h3>Your Task</h3>

You are given the incomplete implementation of the DORMS simulation. Your task is to complete the implementation of the missing classes.

After completing the program, you may run the DORMS simulation:

<pre>$ java Main===== RUNNING SIMULATION =====Mask policy not implemented and SHN not issued===== STATISTICS =====Infected population: 9Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy not implemented and SHN issued===== STATISTICS =====Infected population: 6Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy implemented and SHN not issued===== STATISTICS =====Infected population: 5Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy implemented and SHN issued===== STATISTICS =====Infected population: 4Total Population: 19===== SIMULATION COMPLETED =====</pre>




Or you can run the simulation in verbose mode by issuing the <tt>-v</tt> flag, such as by running <tt>java Main -v</tt>

This task is divided into several levels. Read through all the levels to see how the different levels are related. You are to complete <strong>ALL</strong> levels.

All the files given to you should not be modified. You may modify them for your own testing, but we will replace them to conduct our own tests on your solutions.

<table border="1" cellpadding="10">

 <tbody>

  <tr>

   <td><h4>Level 1</h4><big><strong>Creating the immutable <tt>Virus</tt> class</strong></big>The first thing we’d define is the <tt>Virus</tt> class, because most of the entities of this simulation relies on its implementation.Define an <tt>abstract Virus</tt> class with the following specifications:

    <ul>

     <li>The constructor <tt>Virus(String name, double probabilityOfMutating)</tt> creates a new <tt>Virus</tt> object where it’s name is <tt>name</tt> and probability of mutating upon spreading is <tt>probabilityOfMutating</tt>.</li>

     <li><tt>Virus spread(double random)</tt>. This method causes the virus to spread, returning a new virus. It takes in a <tt>random</tt> value as a <tt>double</tt>. Essentially, if <tt>random &lt;= probabilityOfMutating</tt>, then the virus mutates.</li>

     <li>The string representation of virus objects is shown in the jshell output</li>

    </ul> In addition, define the following concrete classes:

    <ul>

     <li><tt>AlphaCoronavirus</tt>. It’s constructor, <tt>AlphaCoronavirus(double probabilityOfMutating)</tt> initialises it with <tt>name</tt> <tt>"Alpha Coronavirus"</tt>.Calling <tt>spread(random)</tt> returns a <tt>new SARS_CoV_2(this.probabilityOfMutating)</tt> if <tt>random &lt;= this.probabilityOfMutating</tt>, otherwise, it will return a <tt>new AlphaCoronavirus(this.probabilityOfMutating * SimulationParameters.VIRUS_MUTATION_PROBABILITY_REDUCTION)</tt>.</li>

     <li><tt>SARS_CoV_2</tt>. It’s constructor, <tt>SARS_CoV_2(double probabilityOfMutating)</tt> initialises it with <tt>name</tt> <tt>"SARS-CoV-2"</tt>Calling <tt>spread(random)</tt> returns a <tt>new BetaCoronavirus()</tt> if <tt>random &lt;= this.probabilityOfMutating</tt>, otherwise, it will return a new <tt>SARS_CoV_2(this.probabilityOfMutating * SimulationParameters.VIRUS_MUTATION_PROBABILITY_REDUCTION)</tt>.</li>

     <li><tt>BetaCoronavirus</tt>. It’s constructor takes no arguments, and automatically initialises with <tt>0.0</tt> as its <tt>probabilityOfMutating</tt>. When calling <tt>betaCoronavirus.spread(random)</tt>, it will always return a <tt>new BetaCoronavirus()</tt> as it does not mutate.</li>

    </ul> Note the naming, especially with <tt>SARS_CoV_2</tt> where we use a small ‘o’. Note that the name of the virus is spelt with – instead of _; we use _ as the name of the class instead, for obvious reasons.

    <table border="1" width="300">

     <tbody>

      <tr>

       <td><pre>$ jshell -q your_files_in_ascending_dependency_order &lt; test1.jshjshell&gt; new AlphaCoronavirus(0.5)$.. ==&gt; Alpha Coronavirus with 0.500 probability of mutatingjshell&gt; new SARS_CoV_2(0.5)$.. ==&gt; SARS-CoV-2 with 0.500 probability of mutatingjshell&gt; new BetaCoronavirus()$.. ==&gt; Beta Coronavirus with 0.000 probability of mutatingjshell&gt; new AlphaCoronavirus(0.99).spread(0.99)$.. ==&gt; SARS-CoV-2 with 0.990 probability of mutatingjshell&gt; new AlphaCoronavirus(0.99).spread(1)$.. ==&gt; Alpha Coronavirus with 0.891 probability of mutatingjshell&gt; new SARS_CoV_2(0.5).spread(0.5)$.. ==&gt; Beta Coronavirus with 0.000 probability of mutatingjshell&gt; new SARS_CoV_2(0.5).spread(0.51)$.. ==&gt; SARS-CoV-2 with 0.450 probability of mutatingjshell&gt; new BetaCoronavirus().spread(0)$.. ==&gt; Beta Coronavirus with 0.000 probability of mutatingjshell&gt; /exit</pre></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td><h4>Level 2</h4><big><strong>Representing People</strong></big>Now, we are going to implement an <strong>immutable</strong> <tt>Person</tt> class.These are the specifications for the <tt>Person</tt> class:

    <ul>

     <li>The constructor <tt>Person(String name)</tt> creates a <tt>Person</tt> object where his/her name is <tt>name</tt></li>

     <li><tt>List&lt;Virus&gt; transmit(double random)</tt> causes all the viruses in the <tt>Person</tt> to <tt>spread(random)</tt>, where the resulting viruses are stored in a <tt>List</tt>, which is returned. <tt>random</tt> is used to determine if each virus mutates or not.</li>

     <li><tt>Person infectWith(List&lt;Virus&gt; listOfViruses, double random)</tt> returns a new <tt>Person</tt> that represents the person being infected with the <tt>listOfViruses</tt>. <tt>random</tt> is insignificant here because the viruses do not mutate before entering the human body, but only when exiting.</li>

     <li><tt>boolean test(String name)</tt> checks if the <tt>Person</tt> is infected with any <tt>Virus</tt> where its name is <tt>name</tt>.</li>

     <li>The <tt>String</tt> representation is simply the <tt>Person</tt>‘s name.</li>

    </ul>


    <table border="1" width="300">

     <tbody>

      <tr>

       <td><pre>$ jshell -q your_files_in_ascending_dependency_order &lt; test2.jshjshell&gt; Person illio = new Person("Illio");jshell&gt; Person phillmont = new Person("phillmont")jshell&gt; phillmontphillmont ==&gt; phillmontjshell&gt; illio.infectWith(List.of(new AlphaCoronavirus(1)), 0).test("Alpha Coronavirus");$.. ==&gt; truejshell&gt; Arrays.toString(illio.infectWith(List.of(new AlphaCoronavirus(1)), 0).transmit(1).toArray())$.. ==&gt; "[SARS-CoV-2 with 1.000 probability of mutating]"jshell&gt; List&lt;Virus&gt; l = illio.infectWith(List.of(new AlphaCoronavirus(0.5)), 0).transmit(1)jshell&gt; Arrays.toString(l.toArray())$.. ==&gt; "[Alpha Coronavirus with 0.450 probability of mutating]"jshell&gt; Arrays.toString(phillmont.infectWith(l, 1).transmit(1).toArray())$.. ==&gt; "[Alpha Coronavirus with 0.405 probability of mutating]"jshell&gt; Arrays.toString(phillmont.infectWith(l, 1).transmit(0).toArray())$.. ==&gt; "[SARS-CoV-2 with 0.450 probability of mutating]"jshell&gt; /exit</pre></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td><h4>Level 3</h4><big><strong>Making Contact</strong></big>Now we are going to get two people to contact each other and spread viruses.Create an <strong>immutable</strong> <tt>Contact</tt> class that keeps track of a contact between two people, and transmits viruses between them.The following are the specifications of the <tt>Contact</tt> class:

    <ul>

     <li><tt>Contact(Person first, Person second, double time)</tt>. This is the constructor which keeps the references of the two people in contact</li>

     <li><tt>List&lt;Person&gt; transmit(double random)</tt> simultaneously transmits viruses between the two people in the <tt>Contact</tt>, returning a <tt>List</tt> of the resulting <tt>Person</tt> objects. Note that the transmission and infection of viruses happen in parallel, meaning that <tt>a</tt> infects <tt>b</tt> at the same time that <tt>b</tt> infects <tt>a</tt>.For example, person <tt>a</tt> has a <tt>AlphaCoronavirus with 1.000 probability of mutating</tt>.person <tt>b</tt> has a <tt>SARS-CoV-2 with a 1.000 probability of mutating</tt>.After transmission, person <tt>a</tt> would have:

      <ol>

       <li><tt>Alpha Coronavirus with 1.000 probability of mutating</tt></li>

       <li>a newly received <tt>Beta Coronavirus with 0.000 probability of mutating</tt></li>

      </ol>and person <tt>b</tt> would have:

      <ol>

       <li><tt>SARS-CoV-2 with a 1.000 probability of mutating</tt></li>

       <li>a newly received <tt>SARS-CoV-2 with a 1.000 probability of mutating</tt>.</li>

      </ol> </li>

     <li><tt>List&lt;Person&gt; getPeople</tt> this returns the people involved in the <tt>Contact</tt></li>

     <li><tt>double timeOfContact()</tt>. This method simply returns the time of contact</li>

    </ul> Remember that cyclic dependencies are not allowed.

    <table border="1" width="300">

     <tbody>

      <tr>

       <td><pre>$ jshell -q your_files_in_ascending_dependency_order &lt; test3.jshjshell&gt; Person a = new Person("A").infectWith(List.of(new AlphaCoronavirus(1)), 1)jshell&gt; Person b = new Person("B").infectWith(List.of(new SARS_CoV_2(1)), 1)jshell&gt; Contact e = new Contact(a, b, 1)jshell&gt; e.transmit(1).get(0).test("Alpha Coronavirus")$.. ==&gt; truejshell&gt; e.transmit(1).get(0).test("SARS-CoV-2")$.. ==&gt; falsejshell&gt; e.transmit(1).get(0).test("Beta Coronavirus")$.. ==&gt; truejshell&gt; e.transmit(1).get(1).test("Alpha Coronavirus")$.. ==&gt; falsejshell&gt; e.transmit(1).get(1).test("SARS-CoV-2")$.. ==&gt; truejshell&gt; e.transmit(1).get(1).test("Beta Coronavirus")$.. ==&gt; falsejshell&gt; Arrays.toString(e.getPeople().toArray())$.. ==&gt; "[A, B]"jshell&gt; e.timeOfContact()$.. ==&gt; 1.0jshell&gt; /exit</pre></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td><h4>Level 4</h4><big><strong>Locations</strong></big>So far, we’ve only dealt with contact tracing, and have yet to deal with SafeEntry’s cluster tracking. DORMS has specified an <strong>immutable</strong> <tt>Location</tt> class that keeps track of its occupants at any given time.The following defines the specifications for the <tt>Location</tt> class:

    <ul>

     <li><tt>Location(String name)</tt> creates a new empty <tt>Location</tt></li>

     <li><tt>List&lt;Person&gt; getOccupants</tt>. This essentially returns a list of all the occupants in the <tt>Location</tt>.</li>

     <li><tt>Location accept(Person person)</tt>. This returns a new <tt>Location</tt> which represents the original location accepting <tt>person</tt>.</li>

     <li><tt>Location remove(String personName)</tt>. This returns the new <tt>Location</tt> object where the <tt>Person</tt> whose name is <tt>personName</tt> was removed from this <tt>Location</tt>.</li>

     <li>The String representation is simply its name.</li>

    </ul>


    <table border="1" width="300">

     <tbody>

      <tr>

       <td><pre>$ jshell -q your_files_in_ascending_dependency_order &lt; test4.jshjshell&gt; Person mingsoon = new Person("Ming Soon")jshell&gt; Person longThePerson = new Person("Long")jshell&gt; Location l = new Location("LT19")jshell&gt; ll ==&gt; LT19jshell&gt; Arrays.toString(l.getOccupants().toArray())$.. ==&gt; "[]"jshell&gt; Arrays.toString(l.accept(mingsoon)   ...&gt;         .getOccupants().toArray())$.. ==&gt; "[Ming Soon]"jshell&gt; Arrays.toString(l.accept(mingsoon)   ...&gt;         .accept(longThePerson)   ...&gt;         .getOccupants().toArray())$.. ==&gt; "[Ming Soon, Long]"jshell&gt; Arrays.toString(l.accept(mingsoon)   ...&gt;         .accept(longThePerson)   ...&gt;         .remove(mingsoon.toString())   ...&gt;         .getOccupants().toArray())$.. ==&gt; "[Long]"jshell&gt; Arrays.toString(l.accept(mingsoon)   ...&gt;         .accept(longThePerson)   ...&gt;         .remove(mingsoon.toString())   ...&gt;         .remove(longThePerson.toString())   ...&gt;         .getOccupants().toArray())$.. ==&gt; "[]"jshell&gt; /exit</pre></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td><h4>Level 5</h4><big><strong>Mask Policy</strong></big>We are aiming to simulate the efficacy of a mask-wearing policy. As such, we need some way to represent the behaviour of people wearing masks.Hopefully, you have kept your <tt>Person</tt> class open for extension. As such, we can quite simply extend from the <tt>Person</tt> class to create a <tt>MaskedPerson</tt> class, which follows the same specifications.Note that for both transmissions and infections, if the random value supplied is less than or equal to the mask’s effectiveness (see <tt>SimulationParameters#MASK_EFFECTIVENESS</tt>), then nothing is transmitted / infected.The remaining specifications can be inferred from the jshell test. Note that you should adhere to the DRY (Don’t-Repeat-Yourself) principle as much as possible. Expect to make calls to <tt>super</tt> in your overriden methods.

    <table border="1" width="300">

     <tbody>

      <tr>

       <td><pre>$ jshell -q your_files_in_ascending_dependency_order &lt; test5.jshjshell&gt; MaskedPerson frederick = new MaskedPerson("Frederick")jshell&gt; frederick.infectWith(List.of(new AlphaCoronavirus(0.5)), 0.61).test("Alpha Coronavirus")$.. ==&gt; truejshell&gt; frederick.infectWith(List.of(new AlphaCoronavirus(0.5)), 0.6).test("Alpha Coronavirus")$.. ==&gt; falsejshell&gt; Arrays.toString(frederick.infectWith(List.of(new AlphaCoronavirus(0.5)), 0.61).transmit(0.61).toArray())$.. ==&gt; "[Alpha Coronavirus with 0.450 probability of mutating]"jshell&gt; Arrays.toString(frederick.infectWith(List.of(new AlphaCoronavirus(0.5)), 0.61).transmit(0.6).toArray())$.. ==&gt; "[]"jshell&gt; /exit</pre></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td><h4>Level 6</h4><big><strong>Stay-Home-Notice (SHN) Policy</strong></big>We are aiming to simulate the efficacy of an SHN policy. Because DORMS currently does not support this, we need to extend the <tt>Dorms</tt> class to implement this behaviour.Because we need to be able to check if a person is on SHN, DORMS has specified the following method in the <tt>Person</tt> class:

    <ul>

     <li><tt>boolean onSHN(double currentTime)</tt>. This takes in the current time and returns <tt>true</tt> if the <tt>Person</tt> is on SHN.</li>

    </ul>You are also allowed to implement more methods in <tt>Person</tt> and <tt>MaskedPerson</tt> to help you support the SHN issuance. Your job now is to implement the <tt>DormsWithShn</tt> class which is basically DORMS + automatic issuance of SHNs. Remember that you are not allowed to modify the <tt>Dorms</tt> class. Many methods were purposefully declared <tt>final</tt> to prevent extension.Read the remaining classes provided to you carefully. In particular, here are some tips to help you implement the feature:

    <ul>

     <li>The main logic you need to override is <tt>handleSickPerson</tt>. You do not need to override other methods or define other behaviours.</li>

     <li>The constructor for <tt>DormsWithShn</tt> class should be the same as <tt>Dorms</tt>.</li>

     <li>Make use of the <tt>Dorms#queryContacts</tt> method. This retrieves all the <tt>Contact</tt>s that are related to the sick <tt>Person</tt>. This also queries the <tt>Contacts</tt> up to <tt>SimulationParameters#TRACING_PERIOD</tt> in history.</li>

     <li>Because <tt>Person</tt> objects are immutable, <tt>Dorms#updatePerson</tt> updates the state of the <tt>Person</tt> in the <tt>PersonDatabase</tt> and <tt>Dorms#getUpdatedPerson</tt> retrieves the most updated state of a <tt>Person</tt></li>

     <li>Remember to log every SHN served. The format of the output can be seen in the verbose output of the simulation below. Do not use <tt>System.out.println()</tt>, but use <tt>log</tt> instead.</li>

     <li>The behaviour of people checking in or making contact while on SHN is well defined. You do not need to implement this behaviour.</li>

    </ul> Once you’re done with the implementation, you may proceed to run the DORMS simulation by compiling <tt>Main.java</tt> and running <tt>Main</tt> on the JVM. You may also supply the <tt>-v</tt> flag for verbose mode, which shows all your logs.We hope you enjoyed this mock PE, best of luck for next Friday &#x1f642;One final question, do you think DORMS was well designed? How can we improve the design of DORMS?– CS2030/S Teaching Team

    <table border="1" width="300">

     <tbody>

      <tr>

       <td><pre>$ java Main===== RUNNING SIMULATION =====Mask policy not implemented and SHN not issued===== STATISTICS =====Infected population: 9Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy not implemented and SHN issued===== STATISTICS =====Infected population: 6Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy implemented and SHN not issued===== STATISTICS =====Infected population: 5Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy implemented and SHN issued===== STATISTICS =====Infected population: 4Total Population: 19===== SIMULATION COMPLETED =====$ java Main -v===== RUNNING SIMULATION =====Mask policy not implemented and SHN not issuedInitial Disease Carrier visits LT19 at time 0.000Prof Henry visits LT19 at time 0.000Prof Terence visits LT19 at time 0.000Yong Qi visits LT19 at time 0.100Kevin visits LT19 at time 0.110Prof Henry leaves LT19 at time 0.200Sean visits LT19 at time 0.230Yong Qi leaves LT19 at time 0.300Yong Qi met Eric at time 0.330Eric met De Zhang at time 0.340Prof Terence leaves LT19 at time 0.400Prof Henry visits i3 at time 0.450Initial Disease Carrier leaves LT19 at time 0.500Kevin leaves LT19 at time 0.500Sean leaves LT19 at time 0.500De Zhang visits i3 at time 0.900Prof Henry leaves i3 at time 0.950Yong Qi tests positive for SARS-CoV-2 at time 1.000Sean visits i3 at time 1.200Initial Disease Carrier visits i3 at time 1.300De Zhang leaves i3 at time 1.300Initial Disease Carrier leaves i3 at time 1.300Sean leaves i3 at time 1.300Prof Henry met De Zhang at time 1.300Eric met Prof Henry at time 1.400De Zhang tests positive for SARS-CoV-2 at time 3.000Initial Disease Carrier visits COM1-B113 at time 3.100Marcus visits COM1-B113 at time 3.100Jerryl visits COM1-B113 at time 3.200Yong Qi visits COM1-B113 at time 3.300Prof Henry visits COM1-B113 at time 3.400Kevin visits COM1-B114 at time 3.500Destinee visits COM1-B114 at time 3.600Jerryl tests positive for SARS-CoV-2 at time 3.600Yong Qi leaves COM1-B113 at time 3.600Initial Disease Carrier leaves COM1-B113 at time 4.000Marcus leaves COM1-B113 at time 4.000Jerryl leaves COM1-B113 at time 4.000Prof Henry leaves COM1-B113 at time 4.000Kevin leaves COM1-B114 at time 4.000Destinee leaves COM1-B114 at time 4.000Siddarth Raj met Yong Qi at time 5.000Xuan Ming met Yong Qi at time 7.000Xuan Ming visits ION Orchard at time 9.000Jerryl visits ION Orchard at time 9.400Xuan Ming leaves ION Orchard at time 9.500Xuan Ming met Le Yang at time 10.000Le Yang met Joel at time 10.500Mario met Jeremy at time 10.500Bryan visits ION Orchard at time 10.500Geyu visits ION Orchard at time 10.500Jerryl leaves ION Orchard at time 10.500Geyu leaves ION Orchard at time 10.500Bryan leaves ION Orchard at time 10.500Mario test negative for SARS-CoV-2 at time 11.000===== STATISTICS =====Infected population: 9Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy not implemented and SHN issuedInitial Disease Carrier visits LT19 at time 0.000Prof Henry visits LT19 at time 0.000Prof Terence visits LT19 at time 0.000Yong Qi visits LT19 at time 0.100Kevin visits LT19 at time 0.110Prof Henry leaves LT19 at time 0.200Sean visits LT19 at time 0.230Yong Qi leaves LT19 at time 0.300Yong Qi met Eric at time 0.330Eric met De Zhang at time 0.340Prof Terence leaves LT19 at time 0.400Prof Henry visits i3 at time 0.450Initial Disease Carrier leaves LT19 at time 0.500Kevin leaves LT19 at time 0.500Sean leaves LT19 at time 0.500De Zhang visits i3 at time 0.900Prof Henry leaves i3 at time 0.950Yong Qi tests positive for SARS-CoV-2 at time 1.000Yong Qi has been served a SHN that ends at 29.000Initial Disease Carrier has been served a SHN that ends at 14.100Prof Henry has been served a SHN that ends at 14.100Prof Terence has been served a SHN that ends at 14.100Kevin has been served a SHN that ends at 14.110Sean has been served a SHN that ends at 14.230Eric has been served a SHN that ends at 14.330De Zhang leaves i3 at time 1.300De Zhang tests positive for SARS-CoV-2 at time 3.000De Zhang has been served a SHN that ends at 31.000Eric has been served a SHN that ends at 14.340Prof Henry has been served a SHN that ends at 14.900Marcus visits COM1-B113 at time 3.100Jerryl visits COM1-B113 at time 3.200Destinee visits COM1-B114 at time 3.600Jerryl test negative for SARS-CoV-2 at time 3.600Marcus leaves COM1-B113 at time 4.000Jerryl leaves COM1-B113 at time 4.000Destinee leaves COM1-B114 at time 4.000Xuan Ming visits ION Orchard at time 9.000Jerryl visits ION Orchard at time 9.400Xuan Ming leaves ION Orchard at time 9.500Xuan Ming met Le Yang at time 10.000Le Yang met Joel at time 10.500Mario met Jeremy at time 10.500Bryan visits ION Orchard at time 10.500Geyu visits ION Orchard at time 10.500Jerryl leaves ION Orchard at time 10.500Geyu leaves ION Orchard at time 10.500Bryan leaves ION Orchard at time 10.500Mario test negative for SARS-CoV-2 at time 11.000===== STATISTICS =====Infected population: 6Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy implemented and SHN not issuedInitial Disease Carrier visits LT19 at time 0.000Prof Henry visits LT19 at time 0.000Prof Terence visits LT19 at time 0.000Yong Qi visits LT19 at time 0.100Kevin visits LT19 at time 0.110Prof Henry leaves LT19 at time 0.200Sean visits LT19 at time 0.230Yong Qi leaves LT19 at time 0.300Yong Qi met Eric at time 0.330Eric met De Zhang at time 0.340Prof Terence leaves LT19 at time 0.400Prof Henry visits i3 at time 0.450Initial Disease Carrier leaves LT19 at time 0.500Kevin leaves LT19 at time 0.500Sean leaves LT19 at time 0.500De Zhang visits i3 at time 0.900Prof Henry leaves i3 at time 0.950Yong Qi tests positive for SARS-CoV-2 at time 1.000Sean visits i3 at time 1.200Initial Disease Carrier visits i3 at time 1.300De Zhang leaves i3 at time 1.300Initial Disease Carrier leaves i3 at time 1.300Sean leaves i3 at time 1.300Prof Henry met De Zhang at time 1.300Eric met Prof Henry at time 1.400De Zhang tests positive for SARS-CoV-2 at time 3.000Initial Disease Carrier visits COM1-B113 at time 3.100Marcus visits COM1-B113 at time 3.100Jerryl visits COM1-B113 at time 3.200Yong Qi visits COM1-B113 at time 3.300Prof Henry visits COM1-B113 at time 3.400Kevin visits COM1-B114 at time 3.500Destinee visits COM1-B114 at time 3.600Jerryl test negative for SARS-CoV-2 at time 3.600Yong Qi leaves COM1-B113 at time 3.600Initial Disease Carrier leaves COM1-B113 at time 4.000Marcus leaves COM1-B113 at time 4.000Jerryl leaves COM1-B113 at time 4.000Prof Henry leaves COM1-B113 at time 4.000Kevin leaves COM1-B114 at time 4.000Destinee leaves COM1-B114 at time 4.000Siddarth Raj met Yong Qi at time 5.000Xuan Ming met Yong Qi at time 7.000Xuan Ming visits ION Orchard at time 9.000Jerryl visits ION Orchard at time 9.400Xuan Ming leaves ION Orchard at time 9.500Xuan Ming met Le Yang at time 10.000Le Yang met Joel at time 10.500Mario met Jeremy at time 10.500Bryan visits ION Orchard at time 10.500Geyu visits ION Orchard at time 10.500Jerryl leaves ION Orchard at time 10.500Geyu leaves ION Orchard at time 10.500Bryan leaves ION Orchard at time 10.500Mario test negative for SARS-CoV-2 at time 11.000===== STATISTICS =====Infected population: 5Total Population: 19===== SIMULATION COMPLETED ========== RUNNING SIMULATION =====Mask policy implemented and SHN issuedInitial Disease Carrier visits LT19 at time 0.000Prof Henry visits LT19 at time 0.000Prof Terence visits LT19 at time 0.000Yong Qi visits LT19 at time 0.100Kevin visits LT19 at time 0.110Prof Henry leaves LT19 at time 0.200Sean visits LT19 at time 0.230Yong Qi leaves LT19 at time 0.300Yong Qi met Eric at time 0.330Eric met De Zhang at time 0.340Prof Terence leaves LT19 at time 0.400Prof Henry visits i3 at time 0.450Initial Disease Carrier leaves LT19 at time 0.500Kevin leaves LT19 at time 0.500Sean leaves LT19 at time 0.500De Zhang visits i3 at time 0.900Prof Henry leaves i3 at time 0.950Yong Qi tests positive for SARS-CoV-2 at time 1.000Yong Qi has been served a SHN that ends at 29.000Initial Disease Carrier has been served a SHN that ends at 14.100Prof Henry has been served a SHN that ends at 14.100Prof Terence has been served a SHN that ends at 14.100Kevin has been served a SHN that ends at 14.110Sean has been served a SHN that ends at 14.230Eric has been served a SHN that ends at 14.330De Zhang leaves i3 at time 1.300De Zhang tests positive for SARS-CoV-2 at time 3.000De Zhang has been served a SHN that ends at 31.000Eric has been served a SHN that ends at 14.340Prof Henry has been served a SHN that ends at 14.900Marcus visits COM1-B113 at time 3.100Jerryl visits COM1-B113 at time 3.200Destinee visits COM1-B114 at time 3.600Jerryl test negative for SARS-CoV-2 at time 3.600Marcus leaves COM1-B113 at time 4.000Jerryl leaves COM1-B113 at time 4.000Destinee leaves COM1-B114 at time 4.000Xuan Ming visits ION Orchard at time 9.000Jerryl visits ION Orchard at time 9.400Xuan Ming leaves ION Orchard at time 9.500Xuan Ming met Le Yang at time 10.000Le Yang met Joel at time 10.500Mario met Jeremy at time 10.500Bryan visits ION Orchard at time 10.500Geyu visits ION Orchard at time 10.500Jerryl leaves ION Orchard at time 10.500Geyu leaves ION Orchard at time 10.500Bryan leaves ION Orchard at time 10.500Mario test negative for SARS-CoV-2 at time 11.000===== STATISTICS =====Infected population: 4Total Population: 19===== SIMULATION COMPLETED =====</pre></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>