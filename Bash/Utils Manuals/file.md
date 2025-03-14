# file
***
## Basic
Утилита определяет тип файла.
``` bash
file [-bcdEhiklLNnprsSvzZ0] [--apple] [--exclude-quiet] [--extension] [--mime-encoding] [--mime-type] [-e testname] [-F separator] [-f namefile] [-m magicfiles] [-P name=value] file ...

file -C [-m magicfiles]
file [--help]
```
***
## Options
``` bash
--apple # Causes  the file command to output the file type and creator code as used by older MacOS versions.  The code consists of eight letters, the first describing the file type, the latter the creator.  This option works properly only for file formats that have the apple-style output defined.
-b, --brief # Do not prepend filenames to output lines (brief mode).
-C, --compile # Write a magic.mgc output file that contains a pre-parsed version of the magic file or directory.
-c, --checking-printout # Cause a checking printout of the parsed form of the magic file.  This is usually used in conjunction with the -m option to debug a new magic file before installing it.
-d      # Prints internal debugging information to stderr.
-E      # On filesystem errors (file not found etc), instead of handling the error as regular output as POSIX mandates and keep going, issue an error message and exit.
-e, --exclude testname # Exclude the test named in testname from the list of tests made to determine the file type.  
# Valid test names are:
	apptype   # EMX application type (only on EMX).
	ascii     # Various types of text files (this test will try to guess the text encoding, irrespective of the setting of the ‘encoding’ option).
	encoding  # Different text encodings for soft magic tests.
	tokens    # Ignored for backwards compatibility.
	cdf       # Prints details of Compound Document Files.
	compress  # Checks for, and looks inside, compressed files.
	csv       # Checks Comma Separated Value files.
	elf       # Prints ELF file details, provided soft magic tests are enabled and the elf magic is found.
	json      # Examines JSON (RFC-7159) files by parsing them for compliance.
	soft      # Consults magic files.
	simh      # Examines SIMH tape files.
	tar       # Examines tar files by verifying the checksum of the 512 byte tar header.  Excluding this test can provide more detailed content description by using the soft magic method.
	text      # A synonym for ‘ascii’.
--exclude-quiet # Like --exclude but ignore tests that file does not know about.  This is intended for compatibility with older versions of file.
--extension # Print a slash-separated list of valid extensions for the file type found.
-F, --separator separator # Use the specified string as the separator between the filename and the file result returned.  Defaults to ‘:’.
-f, --files-from namefile # Read the names of the files to be examined from namefile (one per line) before the argument list.  Either namefile or at least one filename argument must be present; to test the standard input, use ‘-’ as a  filename  argument.   Please  note that namefile is unwrapped and the enclosed filenames are processed when this option is encountered and before any further options processing is done.  This allows one to process multiple lists of files with different command line arguments on the same file invocation.  Thus if you want to set the delimiter, you need to do it before you specify the list of files, like: “-F @ -f namefile”, instead of: “-f namefile -F @”.
-h, --no-dereference # This option causes symlinks not to be followed (on systems that support symbolic links).  This is the default if the environment variable POSIXLY_CORRECT is not defined.
-i, --mime # Causes the file command to output mime type strings rather than the more traditional human readable ones.  Thus it may say ‘text/plain; charset=us-ascii’ rather than “ASCII text”.
--mime-type, --mime-encoding # Like -i, but print only the specified element(s).
-k, --keep-going # Don't stop at the first match, keep going.  Subsequent matches will be have the string ‘\012- ’ prepended.  (If you want a newline, see the -r option.)  The magic pattern with the highest strength (see the -l option)  comes first.
-l, --list # Shows a list of patterns and their strength sorted descending by magic(5) strength which is used for the matching (see also the -k option).
-L, --dereference # This option causes symlinks to be followed, as the like-named option in ls(1) (on systems that support symbolic links).  This is the default if the environment variable POSIXLY_CORRECT is defined.
-m, --magic-file magicfiles # Specify an alternate list of files and directories containing magic.  This can be a single item, or a colon-separated list.  If a compiled magic file is found alongside a file or directory, it will be used instead.
-N, --no-pad # Don't pad filenames so that they align in the output.
-n, --no-buffer # Force stdout to be flushed after checking each file.  This is only useful if checking a list of files.  It is intended to be used by programs that want filetype output from a pipe.
-p, --preserve-date # On systems that support utime(3) or utimes(2), attempt to preserve the access time of files analyzed, to pretend that file never read them.
-P, --parameter name=value # Set various parameter limits.
	Name         Default    Explanation
    bytes        1M         max number of bytes to read from file
    elf_notes    256        max ELF notes processed
    elf_phnum    2K         max ELF program sections processed
    elf_shnum    32K        max ELF sections processed
    elf_shsize   128MB      max ELF section size processed
    encoding     65K        max number of bytes to determine encoding
    indir        50         recursion limit for indirect magic
    name         50         use count limit for name/use magic
    regex        8K         length limit for regex searches
-r, --raw # Don't translate unprintable characters to \ooo.  Normally file translates unprintable characters to their octal representation.
-s, --special-files # Normally,  file only attempts to read and determine the type of argument files which stat(2) reports are ordinary files.  This prevents problems, because reading special files may have peculiar consequences.  Specifying the -s option causes file to also read argument files which are block or character special files.  This is useful for determining the filesystem types of the data in raw disk partitions, which are block special files.  This option also causes file to disregard the file size as reported by stat(2) since on some systems it reports a zero size for raw disk partitions.
-S, --no-sandbox # On systems where libseccomp (https://github.com/seccomp/libseccomp) is available, the -S option disables sandboxing which is enabled by default.  This option is needed for file to execute  external  decompressing  programs, i.e. when the -z option is specified and the built-in decompressors are not available.  On systems where sandboxing is not available, this option has no effect.
-v, --version # Print the version of the program and exit.
-z, --uncompress # Try to look inside compressed files.
-Z, --uncompress-noreport # Try to look inside compressed files, but report information about the contents only not the compression.
-0, --print0 # Output a null character ‘\0’ after the end of the filename.  Nice to cut(1) the output.  This does not affect the separator, which is still printed. If this option is repeated more than once, then file prints just the filename followed by a NUL followed by the description (or ERROR: text) followed by a second NUL for each entry.
--help # Print a help message and exit.
```
***
## Example
