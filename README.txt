
Backup Scripts
==============

I’m pretty meticulous about backups. I use several rotating off-site
drives for this purpose. It’s critical to test your back ups,
especially for passive bit flips. To accomplish this, I do the
following checks every time I do a full offsite backup (using "rsync"):

1. Full file system check with sector scan:

    ionice -c 3 /sbin/e2fsck -c -f -v -y /device

2. I then run these file checkers:

test_zip – Find all archive (.zip, .gz, etc) files and run the appropriate
           self test on each file.

test_md5 – Find all MD5SUMS files and do a check of all the included files.
           These are simple text files that contain lists of files and their
           MD5 hashs (see the "md5sum" utility for more information).
           They’re useful for capturing the hashes of files that shouldn’t
           change.

test_attr – Tries to hash ALL the files on the specified file system.
            For each file, it stores the file hash and modification time
            in a "extended attribute" in the file’s meta data. When it
            encounters a file that already has this information, it instead
            VERIFIES that the hash has not changed if the modification time
            also didn’t change. This way it can catch transient bit flips
            of any file that supposedly didn’t change (due to modification
            time) but actually did (due to its hash changing).

3. I do some additional application checks. For example, for git repositories,
I do `git fsck`. For my picture collection, I run various checks with my
"hydra" software.

