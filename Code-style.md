CODING CONVENTIONS:
-------------------

*FORMATTING:*

- indenting uses stroustrup style with tabsize 4, i.e. for emacs use in your
	~/.emacs

        (add-hook 'c-mode-common-hook
         (lambda ()
          (show-paren-mode 1)
          (setq indent-tabs-mode t)
          (c-set-style "stroustrup")
          (setq tab-width 4)))

	for vim in ~/.vimrc

        set cindent         " C style indenting
        set ts=4            " tabstop
        set sw=4            " shiftwidth

- for newlines use LF only; avoid CRLF and CR. Git can be configured to convert
  all newlines to LF as source files are commited to the repo by:

        git config --global core.autocrlf input

  (for more information consult http://help.github.com/line-endings/)

- avoid trailing whitespace (spaces & tabs) at end of lines and never use spaces
  for indentation; only ever use tabs for indentations.

    for emacs:

        (add-hook 'before-save-hook 'delete-trailing-whitespace)

    for vim in ~/.vimrc (implemented as an autocmd, use wisely):

        autocmd BufWritePre * :%s/\s\+$//e

- semicolons and commas ;, should be placed directly after a variable/statement

        x+=1;
        set_cache_size(0);

        for (uint32_t i=0; i<10; i++)
            ...

- brackets () and (greater/lower) equal sign ><= should should not contain
  unecessary spaces, e.g:

        int32_t a=1;
        int32_t b=kernel->compute();

        if (a==1)
        {
        }

    exceptions are logical subunits

        if ( (a==1) && (b==1) )
        {
        }

- avoid the use of inline functions where possible (little to zero performance
		impact). nowadays compilers automagically inline code when beneficial
		and within the same linking process

- breaking long lines and strings
	limit yourselves to 80 columns

        for (int32_t vec=params->start; vec<params->end &&
        		!CSignal::cancel_computations(); vec++)
        {
        	//foo
        }

	however exceptions are OK if readability is increased (as in function
	definitions)

- don't put multiple assignments on a single line

- functions look like

        int32_t* fun(int32_t* foo)
        {
        	return foo;
        }

    and are separated by a newline, e.g:

        int32_t* fun1(int32_t* foo1)
        {
        	return foo;
        }

        int32_t* fun2(int32_t* foo2)
        {
        	return foo2;
        }

- same for if () else clauses, while/for loops

        if (foo)
        	do_stuff();

        if (foo)
        {
        	do_stuff();
        	do_more();
        }

- one empty line between { } block, e.g.

        for (int32_t i=0; i<17; i++)
        {
        	// sth
        }

        x=1;

*MACROS & IFDEFS:*

- use macros sparingly
- avoid defining constants using macros (bye bye typechecking), use

        const int32_t FOO=5;

    or enums (when defining several realted constants) instead

- use ifdefs sparingly (really limit yourself to the ones necessary) as their
  extreme usage makes the code completely unreadable. to achieve that it may be
  necessary to wrap a function of (e.g. for
  pthread_create()/CreateThread()/thread_create() a wrapper function to create a
  thread and inside of it the ifdefs to do it the solaris/win32/posix way)
- if you need to use ifdefs always comment the corresponding #else / #endif
  in the following way:


        #ifdef HAVE_LAPACK
          ...
        #else //HAVE_LAPACK
          ...
        #endif //HAVE_LAPACK

*TYPES:*

- types (use only these!):

        char		(8bit char(maybe signed or unsigned))
        uint8_t		(8bit unsigned char)
        uint16_t	(16bit unsigned short)
        uint32_t	(32bit unsinged int)
        int32_t		(32bit int)
        int64_t		(64bit int)
        float32_t	(32bit float)
        float64_t	(64bit float)
        floatmax_t	(96bit or 128bit float depending on arch)

    exceptions: file IO / matlab interface

- classes must be (directly or indirectly) derived from CSGObject

- don't use fprintf/printf, but SG_DEBUG/SG_INFO/SG_WARNING/SG_ERROR/SG_PRINT
  (if in a from CSGObject derived object) or the static SG_SDEBUG/... functions
  elsewise

*FUNCTIONS:*

- Functions should be short and sweet, and do just one thing.  They should fit
  on one or two screenfuls of text (the ISO/ANSI screen size is 80x24, as we all
  know), and do one thing and do that well.
- Another measure of the function is the number of local variables.  They
  shouldn't exceed 5-10, or you're doing something wrong.  Re-think the
  function, and split it into smaller pieces.  A human brain can
  generally easily keep track of about 7 different things, anything more
  and it gets confused.  You know you're brilliant, but maybe you'd like
  to understand what you did 2 weeks from now.

*GETTING / SETTING OBJECTS*

If a class stores a pointer to an object it should call SG_REF(obj) to increase
the objects reference count and SG_UNREF(obj) on class desctruction (which will
decrease the objects reference count and call the objects destructor if
ref_count()==0. Note that the caller (from within C++) of any get_* function
returning an object should also call SG_UNREF(obj) when done with the object.
This makes the swig wrapped interfaces automagically take care of object
destruction.

If a class function returns a new object this has to be stated in the
corresponding swig .i file for cleanup to work, e.g. if apply() returns a new
CLabels then the .i file should contain `%newobject CClassifier::apply();`

*NAMING CONVENTIONS:*

- naming variables:
	- in classes are member variables are named like m_feature_vector (to avoid
		shadowing and the often hard to find bugs shadowing causes)
	- parameters (in functions) shall be named e.g. feature_vector
	- don't use meaningless variable names, it is however fine to use short names
	like i,j,k etc in loops
	- class names start with 'C', each syllable/subword starts with a capital letter,
	  e.g. CStringFeatures

- constants/defined objects are UPPERCASE, i.e. REALVALUED

- function are named like get_feature_vector() and should be limited to as few arguments as
  possible (no monster functions with > 5 arguments please)

- objects which can deal with features of type DREAL and class SIMPLE don't need
  to contain Real/Dense in class name

- others are required to clarify class/type they can handle, e.g.
  CSparseByteLinearKernel, CSparseGaussianKernel


- variable and function names are all lowercase (except for class Con/Destructors)
  syllables/subwords are separated by '_', e.g. compute_kernel_value(), my_local_variable

- class member variables all start with m_, e.g. m_member (this is to avoid shadowing)

- features and preprocessors are prefixed with featureclass (e.g. Dense/Sparse) followed by featuretype (Real/Byte/...)