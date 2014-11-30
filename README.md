android-aptlibs-gradle-plugin
=============================

Very easy way to add annotation processors to your andorid project. 

aptlibs provide 3 predefined libs: android annotations, annotated sql, groundy.

	ext.asVersion = '1.10.0'
	ext.aaVersion = '3.0.1'
	ext.groundyVersion = '1.5'

	aptlibs {
		annotatedSql {
			version "${asVersion}"
		}

		groundy {
			version "${groundyVersion}"
		}

		androidAnnotations {
			version "${aaVersion}"
		}
	}
	

unfortunately you still need to add annotations(without processor) to project dependecies 

	dependencies {
		compile "com.github.hamsterksu:android-annotatedsql-api:${asVersion}"
		compile "com.telly:groundy:${groundyVersion}"
		compile "org.androidannotations:androidannotations-api:${aaVersion}"
	}

##additional gradle config:##

####add build dependecies####

	buildscript {
		repositories {
			mavenCentral()
		}
		dependencies {
			classpath 'com.github.hamsterksu:android-aptlibs-gradle-plugin:1.2.0'
		}
	}

####add plugin like####

	apply plugin: 'android'
	apply plugin: 'aptlibs'

##Add custom processor to aptlibs##

Sure every lib is optional.

Also you can add custom processor:

	aptlibs {
		custom{
			customAndroidAnnotations {
				processors = ["com.googlecode.androidannotations.AndroidAnnotationProcessor"]
				groupId 'com.googlecode.androidannotations'
				artifactIdApt 'androidannotations'
				artifactIdLibrary 'androidannotations-api'
				version "${aaVersion}"

				customArgs { variant ->
					arg "-AandroidManifestFile", "${variant.processResources.manifestFile}"
				}
			}
		}
	}

### AndroidAnnotation - fix issue with new package name

So if you want to have different applicationId for different productFlavors you need to tune AA processor. you need to add -AresourcePackageName argument


	aptlibs {
		androidAnnotations{
			version "${aaVersion}"

			customArgs { variant ->
				arg "-AandroidManifestFile", "${variant.processResources.manifestFile}"
				arg "-AresourcePackageName", <base(main flavour) package name here>
				arg "-AlogAppenderConsole", "true"
			}
		}
	}


