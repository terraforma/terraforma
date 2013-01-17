General
-------
**Use real tabs that equal 4 spaces.**

**Use typically trailing braces everywhere** (if, else, functions, structures, typedefs, class definitions, etc.)

	if (x) {
	}

**The else statement starts on the same line as the last closing brace.**

	if (x) {
	} else {
	}

**Don’t pad parenthesized expressions with spaces**

	if (x) {
	}

Instead of

	if ( x ) {
	}

And

	x = (y * 0.5f);

Instead of

	x = ( y * 0.5f );

**Use precision specification for floating point values unless there is an explicit need for a double.**

	float f = 0.5f;

Instead of

	float f = 0.5;

And


	float f = 1.0f;
Instead of

	float f = 1.f;

**Function names start with an upper case:**

	void Function();

**In multi-word function names each word starts with an upper case:**

	void ThisFunctionDoesSomething();

**The standard header for functions is:**

	/*
	====================
	FunctionName
	
	 Description
	====================
	*/

**Variable names start with a lower case character.**

	float x;

**Argument names use an underscore prefix**

	void Function(int _argument);

Instead of

	void Function(int argument);

**In multi-word variable names the first word starts with a lower case character and each successive word starts with an upper case.**

	float maxDistanceFromPlane;

**Typedef names start with an upper case character, as does each successive word. They always end with "_t".**

	typedef int FileHandle_t;

**Struct names use the same naming convention as Typedef names**

	struct RenderEntity_t;

**Enum names use the same naming convention as Typedef names. The enum constants use all upper case characters. Multiple words are separated with an underscore.**

	enum Contact_t {
		CONTACT_NONE,
		CONTACT_EDGE,
		CONTACT_MODELVERTEX,
		CONTACT_TRMVERTEX
	};

**Names of recursive functions end with "_r"**

	void WalkBSP_r(int _node);

**Defined names use all upper case characters. Multiple words are separated with an underscore.**

	#define SIDE_FRONT		0

**Use ‘const’ as much as possible.**

Use:

	const int *p;			// pointer to const int
	int * const p;			// const pointer to int
	const int * const p;		// const pointer to const int

Don’t use:

	int const *p;

**Include the project name, and full source file path in include guards.**

Eg. In file include/Version.h in the libterra project:

	#ifndef _LIBTERRA_INCLUDE_VERSION_H_
	#define _LIBTERRA_INCLUDE_VERSION_H_

	#endif /* _LIBTERRA_INCLUDE_VERSION_H_ */ 

**Use a namespace pertaining to the project you're working in**

In the libterra project:

	namespace LibTerra {
		...
	}

Classes
-------
**The standard header for a class is**:

	/*
	===========================================================================
		Class Name
	
		Description

	===========================================================================
	*/

**Class names start with "tf"**

Each successive word starts with an upper case character

	class tfVec3;

**Private member variables use an "m_" prefix**

Each successive word starts with an upper case character

	class tfCharacter {
	private:
		const char *	m_name;
		const char *	m_meshFile;
	}

**Class methods have the same naming convention as functions.**

	class tfVec3 {
		float		Length() const;
	}

**Indent the names of class variables and class methods to make nice columns.**

The variable type or method return type is in the first column and the variable name or method name is in the second column.

	class tfVec3 {
		float		m_x;
		float		m_y;
		float		m_z;
		float		Length() const;
		const float *	ToFloatPtr() const;
	}

The * of the pointer is in the first column because it improves readability when considered part of the type.

**Ordering of class variables and methods should be as follows**:

1. list of friend classes
2. public variables
3. public methods
4. protected variables
5. protected methods
6. private variables
7. private methods

This allows the public interface to be easily found at the beginning of the class.

**Always make class methods ‘const’ when they do not modify any class variables.**

**Avoid use of ‘const_cast’.**

When object is needed to be modified, but only const versions are accessible, create a function that clearly gives an editable version of the object.  This keeps the control of the ‘const-ness’ in the hands of the object and not the user.

**Return ‘const’ objects unless the general usage of the object is to change its state.**

 For example, media objects like idDecls should be const to a majority of the code, while idEntity objects tend to have their state modified by a variety of systems, and so are ok to leave non-const.

**Function overloading should be avoided in most cases.**

For example, instead of:

	const tfAnim *	GetAnim(int _index) const;
	const tfAnim *	GetAnim(const char *_name) const;
	const tfAnim *	GetAnim(float _randomDiversity) const;

Use:

	const tfAnim *	GetAnimByIndex(int _index) const;
	const tfAnim *	GetAnimByName(const char *_name) const;
	const tfAnim *	GetRandomAnim(float _randomDiversity) const;

**Explicitly named functions tend to be less prone to programmer error and inadvertent calls to functions due to wrong data types being passed in as arguments.**

Example:

	Anim = GetAnim(0);

This could be meant as a call to get a random animation, but the compiler would interpret it as a call to get one by index.

**Overloading functions for the sake of adding ‘const’ accessible function is allowable**:

	class tfAnimatedEntity : public tfEntity {
		tfAnimator * 		GetAnimator();
		const tfAnimator * 	GetAnimator() const;
	};

In this case, a const version of GetAnimator was provided in order to allow GetAnimator to be called from const functions.  Since idAnimatedEntity is normally a non-const object, this is allowable.  For a media type, which is normally const, operator overloading should be avoided:

	class tfDeclMD5 : public tfDecl {
		const tfMD5Anim *	GetAnim(animHandle_t _handle) const;
		tfMD5Anim *		GetEditableAnim(animHandle_t _handle);
	};

File Names
----------

**Each class should be in a separate source file unless it makes sense to group several smaller classes.**

**The file name should be the same as the name of the class without the "tf" prefix.**

(Upper/lower case is preserved.)

	class tfWinding;
	
	Winding.cpp
	Winding.h

**When a class spans across multiple files these files have a name that starts with the name of the class without "tf", followed by an underscore and a subsection name.**

	class tfRenderWorld;
	
	RenderWorld_load.cpp
	RenderWorld_demo.cpp
	RenderWorld_portals.cpp

**When a class is a public virtual interface to a subsystem, the public interface is implemented in a header file with the name of the class without "tf".**

The definition of the class that implements the subsystem is placed in a header file with the name of the class without "tf" and ends with "_local.h". The implementation of the subsystem is placed in a cpp file with the name of the class without "tf".

	class tfRenderWorld;

	RenderWorld.h			// public virtual tfRenderWorld interface
	RenderWorld_local.h		// definition of class tfRenderWorldLocal
	RenderWorld.cpp			// implementation of tfRenderWorldLocal

