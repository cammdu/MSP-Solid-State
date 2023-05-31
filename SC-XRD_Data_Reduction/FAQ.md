### Can I use Mdx2 without using DIALS first?
It is possible but not advised as not all functionalities have been implemented with Mdx2, so you won't be able to index reliably as DIALS does. 

### Why do I get an infinity error in the integration tutorial?
That's propbably because you haven't set your exposure time >0 in refined.expt. Change it with the for loop I include in the DIALS tuorialn and it should work now

### How do I know which indexing to assign?
Well, firstly, you should know enough about your material to know which point group its structure is in. Secondly, DIALS produces very detailed suggestion tables regarding the correct space group for your structure as I explain in the DIALS tutorial. So use your own head and DIALS algorithm 

### Do I need to know python?
The short answer is yes, you'll need python for meerkat as well as to constantly check whether your outputs are correct. Python is also a great tool for personalised plotting. 

### How much space should I have on my computer?
A lot. If you're only processing one dataset and have python and the software installed already, you should have around 30 gb available. 

### Are there any statistical plots available?
Yes, lots actually. If you open the report that DIALS provides you'll get a lot of plots.
