# What is gagarhive

gagarchive creates immutable file archives which can help you to optimize cloud file storage costs.

Popular file storages like AWS S3 bills you for both storage you use and requests you make. Imagine you want to store some kind of small log files and you can't logically concatenate them, and you have a lot of them. Moreover, sometimes you want to read some of them without downloading full archive.

> Example: 1T of files with average size of 10Kb. This is ~100k of PUT requests to make and ~$500 to pay only for these requests.

You can gzip them and easily go from $500 to $0, and this is a very good idea unless you want to read some of these files in the future without downloading 1Tb of data. 

# How gagarchive helps in this case?

Gagarchive archive consists of 2 files: contents and index. Contents file is simply concatenated file bodies. Index file contains filenames of files in archive and their contents offset from the beginning of the contents file.

So, in order to read specific file from the archive, gagarchive finds it in the index file, and reads it from contents file by using offset it found. Popular file storages often allow you to issue GET requests with Range header, which helps to avoid downloading 1Tb file to get 10k file.

> Example. 1T of files with average size of 10Kb archived in 100Mb chunks. This is ~10k of PUT requests to make and ~$50 to pay only for these requests. And now if you want to find some file, you need to read only (some) index files for it. 

It's up to you where to store (in clouds or locally) and how to logically group index files. Usually it depends on how often you want to read from your archives.
