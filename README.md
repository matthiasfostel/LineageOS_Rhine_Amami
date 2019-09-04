# Instructions assume following system requirements.

Ubuntu 16 LTS

If virtualized, more CPUs will make the compile faster, but do not exceed total in system.

8 GB RAM minimum

200 GB hard drive minimum

root access

# Initial Build

sudo -s

apt-get install bc bison build-essential curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline6-dev lib32z1-dev libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev openjdk-8-jdk

mkdir -p ~/bin

mkdir -p ~/android/lineage

curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

chmod a+x ~/bin/repo

cd ~/android/lineage

source ~/.profile

repo init -u https://github.com/LineageOS/android.git -b cm-14.1

> Repo init might fail asking for git email and username, feel free to copy/paste the commands from warning without any changes

repo sync

> This might take awhile (45 minutes+) depending on Internet speed and system specs. If building in a virtual machine, you might want to consider creating a snapshot when the first repo sync completes and perhaps a second after your first compile finishes (after the make bacon below).

mkdir .repo/local_manifests

curl https://raw.githubusercontent.com/matthiasfostel/LineageOS_Rhine_Amami/14.1/roomservice.xml > ~/android/lineage/.repo/local_manifests/roomservice.xml

repo sync --force-sync

export USE_CCACHE=1

export ANDROID_JACK_VM_ARGS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx7G"

> You might want to add the CCACHE and JACK exports above to your ~/.bashrc

prebuilts/misc/linux-x86/ccache/ccache -M 50G

. build/envsetup.sh

lunch lineage_amami-userdebug

make bacon -j4

> Lineage/Android will compile and create a firmware ZIP that should be found in:
>
> ~\android\lineage\out\target\product\amami named lineage-14.1-DATE-UNOFFICIAL-SonyLOS-amami.zip

# To Update Builds run the following

sudo -s

rm -rf ~/.jack-server

> Make sure to save a backup of the firmware zip elsewhere before next command.

rm -rf ~/android/lineage/out

cd ~/android/lineage

curl https://raw.githubusercontent.com/matthiasfostel/LineageOS_Rhine_Amami/14.1/roomservice.xml > ~/android/lineage/.repo/local_manifests/roomservice.xml

repo sync --force-sync

. build/envsetup.sh

lunch lineage_amami-userdebug

make bacon -j4

# Special Thanks

Big thank you to the LineageOS, Sonyxperiadev and SonyLOS teams, as well as derfelot and Micha-Btz.
