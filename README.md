# Command Line Interface (CLI) & OpenSSL Engine for OPTIGA™ Trust M1 security solution

1. [About](#about)
    * [Prerequisites](#prerequisites)
    * [Contents of the package](#contents_of_package)
2. [Getting Started](#getting_started)
    * [First time building the library](#build_lib)
    * [Building the engine](#build_engine) 
3. [CLI Tools Usage](#cli_usage)
    * [trustm_chipinfo](#chipinfo)
4. [Trust M1 OpenSSL Engine usage](#engine_usage)
    * [rand](#rand)
    * [req](#req)
    * [pkey](#pkey)
    * [dgst](#dgst)
5. [Known issues](#known_issues)

## <a name="about"></a>About

    This is a command line tools tools and OpenSSL Engine for OPTIGA Trust M1 on Linux platform.


### <a name="prerequisites"></a>Prerequisites

    Following is the software component to build the tools :
	- GCC
	- OpenSSL development library (libssl-dev)
	- OpenSSL 1.1.X
	- OPTIGA Trust M1 library (source code)
	- wiringPI
	- pthread
	- rt

    Tested platforms:
      - Raspberry PI 3 on Linux kernal 4.19

### <a name="contents_of_package"></a>Contents of Package

    This tools consists of the following files & directory:
	.
	├── bin				/* all executable amd .so file is store here	 */
	├── LICENSE                     // MIT license file
	├── linux_example               // Source code for executable file
	│   └── trustm_chipinfo.c       // source to list chip info
	├── Makefile                    // this project Makefile 
	├── README.md                   // this read me file in Markdown format 
	├── trustm_engine               /* all trust M1 OpenSSL Engine source code       */
	│   ├── trustm_engine.c         // entry point for Trust M1 OpenSSL Engine 
	│   ├── trustm_engine_common.h  // header file for Trust M1 OpenSSL Engine
	│   ├── trustm_engine_rand.c    // Random number generator source  
	│   └── trustm_engine_rsa.c     // RSA source 
	├── trustm_helper		/* Helper rountine for trust M library           */
	│   ├── include			/* Helper include directory                     
	│   │   └── trustm_helper.h	// Helper header file
	│   └── trustm_helper.c		// Helper source 
	└── trustm_lib                  // Directory for trust M library

## <a name="getting_started"></a>Getting Started
### <a name="build_lib"></a>First time building the library
    - sudo make install_debug_lib 
      or 
    - sudo make install_lib

### <a name="build_engine"></a>Building the engine
    - sudo make install_debug_engine 
      or 
    - sudo make install_engine

Note:
- If debug is used subsequence building just do a ‘make’ as the Makefile creates a 
  soft-link to the bin directory
- If without debug than every time you build the library or engine you must reinstall

## <a name="cli_usage"></a>CLI Tools Usage
### <a name="chipinfo"></a>trustm_chipinfo
    Display the trustm chip information.

## <a name="engine_usage"></a>Trust M1 OpenSSL Engine usage
### <a name="rand"></a>rand
    Usuage : Random number generation
    Example 
        $openssl rand -engine trustm_engine -base64 1024
    Note : 
    If trustM random number generation fails, there will still be random number output. 
    This is control by openSSL engine do not have control over it.

### <a name="req"></a>req
    Usuage : Certificate request / self signed cert / key generation
    Example
        $openssl req -keyform engine -engine trustm_engine -key <OID>:<Public key file | *>:<NEW> -new -out test.csr -verify
    Note:
    If wrong public is used or no pubkey is submitted the certificate generation will still 
    go through but verification will fail. Pubic key input only in PEM

### <a name="pkey"></a>pkey
    Usuage : Key tools / Key generation
    Example
        $openssl pkey -engine trustm_engine -pubout -inform engine -in 0xe0fc:*:NEW -out testpube0fc.pem

### <a name="dgst"></a>4.4 dgst
    Usuage : Sign and verify
    Example
        $openssl dgst -sign 0xe0fc -engine trustm_engine -keyform engine -out helloworld.sig helloworld.txt
        $openssl dgst -engine trustm_engine -verify testpube0fc.pem -keyform engine -signature helloworld.sig helloworld.txt

## <a name="known_issues"></a>5. Known issues
