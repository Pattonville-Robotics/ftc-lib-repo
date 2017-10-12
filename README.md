# ftc-lib-repo

To use, paste this in your `repositories` block
```Gradle
maven {
    url "https://raw.githubusercontent.com/Pattonville-Robotics/ftc-lib-repo/mvn-repo/"
}
```
and then paste this to replace the `.jar` dependencies
```Gradle
compile 'org.first.ftc:inspection:3.4'
compile 'org.first.ftc:blocks:3.4'
compile 'org.first.ftc:robotcore:3.4'
compile 'org.first.ftc:hardware:3.4'
compile 'org.first.ftc:ftc-common:3.4'
compile 'org.first.ftc:analytics:3.4'
compile 'org.first.ftc:wireless-p2p:3.4'
```
