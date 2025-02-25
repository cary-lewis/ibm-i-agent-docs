# Dynamic Variable General Rules and Suggestions

Examples of where dynamic variables may be used are offered throughout
this online help in each topic. There is more information about dynamic
variables in the Screens and Windows section of this topic. Here are
some general rules and suggestions to keep in mind when planning to use
Dynamic Variables:

- Dynamic Variables and the tokens derived from them are different
    from the Variables/Tokens used in Operator Replay scripts. These two
    different LSAM variable types are used for different purposes as of
    this version of the LSAM, and they cannot be interchanged. However,
    it is possible to transfer values from Dynamic Variables to Operator
    Replay Tokens.
- The dynamic variable type of L is used exclusively to update the
    local data area (LDA) contents associated with tracked, queued or
    captured jobs. Dynamic variables of type L replace data in the LDA
    based on the starting position and length fields specified in the
    dynamic variable master record. These two numeric fields apply only
    to type L variables and then cannot be used for type V variables.
    For dynamic variables of type L, the Variable Name must be either
    the Captured Job ID of a captured job, or the IBM i Job Name of a
    tracked or queued job.
- The dynamic variable type of V cannot be used to update LDA content.
    Instead, this variable type is used to define tokens that can be
    inserted into the job definition parameters or the call command
    string of a captured job. Type V dynamic variables may have any
    meaningful name. One purpose for dynamic variable tokens is to
    insert them into the capture job definitions using the WRKCAPJOB
    function. However, there also exist many other uses for dynamic
    variable tokens.
- It is possible for dynamic variable tokens to appear, for example,
    in the Call command string of any batch job submitted by OpCon. This
    is made possible by the rules of LSAM dynamic variables that are
    different from the rules for OpCon variables/tokens made from OpCon
    properties. OpCon job processing recognizes a pair of dual straight
    or curly brackets: \[\[name\]\] or {{name}} as an OpCon variable,     but it ignores the single pair of curly brackets: {VARNAME} that is
    used to build an LSAM dynamic variable token.
- All dynamic variable value substitution processing for submitted
    jobs is performed by a sub-procedure of the LSAM job scheduler
    server program. Value substitution takes place as a last step just
    before the SBMJOB command string is actually executed to start the
    job. The only variation to this method is that dynamic variables
    will also be honored by the LSAM procedures that support an operator
    manually releasing a job that was held for tracking or queuing.
- There are four ways to specify the value used for a dynamic
    variable:
  - A fixed value may be specified in the VALUE field of the dynamic
        variable master record.
  - A user-defined program may be specified in the dynamic variable
        master record. In this case, if the user-defined program is not
        found at run time, a specified fixed value will be used as the
        default in place of the program-supplied value. User-defined
        programs must accept and return at least one character string
        parameter of any length from 1 to 128 characters. (The name of
        the Dynamic Variable is also provided as parameter 2, and in
        parameter 1 the current value of the Dynamic Variable is sent to
        the user-defined program.) If the user-defined program returns
        all blanks in the value parameter, the blanks will be accepted
        as the value and the effect of this is that the dynamic variable
        token will be removed from its location in the command string
        and the command string will be compressed to remove the
        left-over spaces.
  - The LSAM command SETDYNVAR may be used to add a new dynamic
        variable or to change any of the existing parameters that define
        a dynamic variable. Typically, this command could be used to set
        the fixed value of the dynamic variable just before the variable
        is used (The SETDYNVAR command parameters are defined under the
        next major heading of this topic, below).
  - Captured Data Response Rules support naming a Dynamic Variable
        that will be used to store the value of the currently captured
        data, from either Operator Replay screen capture or the SCANSPLF
        command Scan Rules. These rules also make reference to Dynamic
        Variable tokens in their Response Command field and their
        comparison data fields.
- An excellent use of the LSAM command SETDYNVAR would be to execute
    this command in a separate job of an OpCon schedule, just before
    executing the job that depends on a dynamic variable. Similarly, the
    SETDYNVAR command can be specified as the pre-run command for an
    OpCon job sent to IBM i.
- An interesting application of the SETDYNVAR command used in OpCon
    job definitions would be to insert an OpCon property token into the
    command string where the SETDYNVAR VALUE parameter is specified.
    This would allow any property value computed by OpCon to be
    transmitted to the LSAM, where it could then become part of a job's
    LDA content, used as a SBMJOB command parameter value or used to
    modify the Call command string of the job. Here are examples of how
    the command could be set up to utilize OpCon properties:
