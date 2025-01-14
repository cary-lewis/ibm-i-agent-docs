# Dynamic Variables

Important information about the definition and application of Dynamic
Variables is also presented in the description of the SETDYNVAR command,
in this section, and in Screens and Windows, below.

The LSAM database includes a variable field definition table file with
two variable types that can be used for many purposes. Dynamic variables
of type L are used to update the stored LDA content for tracked, queued
or captured jobs. Dynamic variables of type V are used to update the job
parameters of any job submitted to the LSAM by OpCon. The type V
variables are also supported by various fields of Spool File Scan Rules
and of Captured Data Response Rules, as well as by other LSAM functions
(documented within other topics of this online help). Each of these
variable field definitions has a token name to identify them, but the
token name of type L variables must match either the job name of tracked
and queued jobs or the captured job ID of captured jobs.

Dynamic variable values are always applied to a job at the last moment
before the LSAM submits the job for execution. When the LSAM debug
logging function is turned on, the submitted job log file (option 4 in
the LSAM's log file viewer menu) will report on actions performed by
dynamic variables. It is possible to see in that log file the updated
LDA content and also the before- and after-versions of the SBMJOB
command string.

Similarly, dynamic variables used with Captured Data Response Rules are
interpreted at the time when data is captured, for example, as the
SCANSPLF command is executing or as Operator Replay screen data or
Message Management data is being captured. As each data element is being
stored to the Captured Data log file, the system checks for Captured
Data Response Rules to process. Dynamic variables may be part of the
comparison rule that allows a Captured Data Response to be executed, and
they can also comprise all or part of the command that is executed.

Regardless of the purpose of the dynamic variable, the value inserted to
replace the LSAM's dynamic variable can be either a fixed value that is
stored in the dynamic variables master file record or it can be a value
produced by a user-defined program that has been registered in the
dynamic variable's master record. When the fixed value approach is
used, that fixed value may actually be updated at any time by the
LSAM's SETDYNVAR or SETCAPVAR commands. These commands present some
interesting possibilities.

It is possible, for example, to use a separately scheduled OpCon job or
the pre-run command line of an OpCon job for IBM i to execute the
SETDYNVAR command just before another job is executed that will depend
on that dynamic variable. It is also possible to include OpCon property
tokens in the VALUE parameter of the SETDYNVAR command so that a value
calculated by OpCon can be passed along to the LSAM, and the final
result is that a job executing under IBM i can have its behavior
controlled by virtually any OpCon property.

Separate from the value controls that apply to all dynamic variables,
the dynamic variable type field determines how the value is applied to
attributes of a job.

Dynamic variables of type L are used specifically for updating the
stored LDA contents associated with a tracked, queued or captured job.
This type of variable must use the Tracked (or Queued) Job Name or the
Captured Job ID as the Variable Name in the dynamic variable master
file. There are dynamic variable parameters used only for this type of
variable that specify the location and length of the LDA content that
should be updated. The other dynamic variable type V cannot be used to
update LDA contents, and the start position and length fields cannot be
used with type V variables.

Dynamic variables of type V can be used, among other purposes, to update
any part of the total command string that the LSAM uses to submit jobs.
This means that, depending on how and where a job is being defined (that
is, by using OpCon job master records or by using the Capture Job
function and maintenance), it is possible all command parameters may be
optionally included, or just the values of single command parameter may
be updated. It is also possible to modify the content of the submitted
job's command line using type V dynamic variables.

To use type V dynamic variables, the Variable Name value is enclosed
within a pair of designated control characters to form a token. This
token is inserted anywhere in the submitted job's command line. At the
point where the LSAM has assembled the total SBMJOB command, it performs
a scan for any dynamic variable tokens and then replaces those tokens
with the value produced by the dynamic variable. (The processing of
dynamic variables used with Capture Data Response Rules is described in
[Operator Replay Scripts](/operator-replay/overview)
, in [Message Management](/message-management/overview)
, and elsewhere in this Events and Utilities menu in relation to
the SCANSPLF command.)

The special characters used to make tokens out of type V dynamic
variables are specified in the LSAM's Job Tracking configuration
function, from the LSAM menu system. In most cases, it should not be
necessary to modify these characters. A single pair of curly brackets {}
have been chosen as the default control characters for dynamic variable
tokens because this convention permits the dynamic variable token to
pass transparently through the different property (variable, token)
replacement logic used by OpCon. The LSAM Events and Utilities menu
option 7: LSAM Utility configuration provides a means to change these
characters in order to accommodate translation table conflicts that
might arise in some countries where rules that work well for English or
other Western language translation tables may not apply.
