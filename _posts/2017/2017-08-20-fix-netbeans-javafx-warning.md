---
title: Fix the javafx Warning
---

# 警告信息

> Missing Codebase manifest attribute for:xxx.jar

在 build.xml 文件添加以下代码:

    <target name="-post-jfx-jar">
        <update-libs-manifest />
        <update-jar-manifest jarfile="${jfx.deployment.dir}${file.separator}${jfx.deployment.jar}" />
    </target>   

    <macrodef name="update-libs-manifest">
        <sequential>
            <property name="pp_rebase_dir" value="${basedir}${file.separator}${dist.dir}${file.separator}lib"/>
            <property name="pp_rebase_fs" value="*.jar"/>

            <condition property="manifest.codebase" value="${manifest.custom.codebase}" else="*">
                <and>
                    <isset property="manifest.custom.codebase" />
                    <not>
                        <equals arg1="${manifest.custom.codebase}" arg2="" trim="true" />
                    </not>
                </and>
            </condition>

            <condition property="manifest.permissions" value="all-permissions" else="sandbox">
                <isset property="permissions-elevated" />
            </condition>

            <echo message="Updating libraries manifest => Codebase: ${manifest.codebase} / Permissions: ${manifest.permissions}" />
            <script language="javascript">
                <![CDATA[
                    var dir = project.getProperty("pp_rebase_dir");
                    var fDir = new java.io.File(dir);
                    if( fDir.exists() ) {
                        var callTask = project.createTask("antcall");
                        callTask.setTarget("-update-jar-macro-call");
                        var param = callTask.createParam();
                        param.setName("jar.file.to.rebase");
                        var includes = project.getProperty("pp_rebase_fs");
                        var fs = project.createDataType("fileset");
                        fs.setDir( fDir );
                        fs.setIncludes(includes);
                        var ds = fs.getDirectoryScanner(project);
                        var srcFiles = ds.getIncludedFiles();
                        for (i=0; i<srcFiles.length; i++) {
                            param.setValue(dir + "${file.separator}" + srcFiles[i]);
                            callTask.perform();
                        }
                    }
                ]]>
            </script>
        </sequential>
    </macrodef>

    <target name="-update-jar-macro-call">
        <update-jar-manifest jarfile="${jar.file.to.rebase}"/>
    </target>

    <macrodef name="update-jar-manifest">
        <attribute name="jarfile"/>
        <sequential>
            <echo message="jarfile = @{jarfile}" />
            <jar file="@{jarfile}" update="true">
                <manifest>
                    <attribute name="Codebase" value="${manifest.codebase}"/>
                    <attribute name="Permissions" value="${manifest.permissions}"/>
                </manifest>
            </jar>
        </sequential>
    </macrodef>

之后，只需要在 project.properties 文件添加：
    manifest.custom.codebase=<your_codebase>

例如: 
    manifest.custom.codebase=localhost

注意：本代码仅适用于 Netbeans 默认的 ANT 构建过程。你需要按照你的需要更新它，根据需要改变 target(<target name="-post-jfx-jar">)，属性名和权限检查条件。
