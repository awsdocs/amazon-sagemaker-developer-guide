# Clean Up Amazon SageMaker Experiment Resources<a name="experiments-cleanup"></a>

To avoid incurring unnecessary charges, delete the Amazon SageMaker Experiment resources you no longer need\. You can't delete Experiment resources through the SageMaker Management Console or the Amazon SageMaker Studio UI\. This topic shows you how to clean up these resources using the SageMaker Python SDK, Boto3, and the Experiments SDK\.

**Topics**
+ [Clean Up Using the SageMaker Python SDK](#experiments-cleanup-smpythonsdk)
+ [Clean Up Using the Python SDK \(Boto3\)](#experiments-cleanup-boto3)
+ [Clean Up Using the Experiments SDK](#experiments-cleanup-smsdk)

## Clean Up Using the SageMaker Python SDK<a name="experiments-cleanup-smpythonsdk"></a>

**To clean up using the SageMaker Python SDK**

```
from sagemaker.experiments.experiment import _Experiment

exp = _Experiment.load(experiment_name=experiment_name, sagemaker_session=sm_session)
exp._delete_all(action="--force")
```

## Clean Up Using the Python SDK \(Boto3\)<a name="experiments-cleanup-boto3"></a>

**To clean up using Boto 3**

```
import boto3
sm = boto3.Session().client('sagemaker')
```

**Define cleanup\_boto3**

```
def cleanup_boto3(experiment_name):
    trials = sm.list_trials(ExperimentName=experiment_name)['TrialSummaries']
    print('TrialNames:')
    for trial in trials:
        trial_name = trial['TrialName']
        print(f"\n{trial_name}")

        components_in_trial = sm.list_trial_components(TrialName=trial_name)
        print('\tTrialComponentNames:')
        for component in components_in_trial['TrialComponentSummaries']:
            component_name = component['TrialComponentName']
            print(f"\t{component_name}")
            sm.disassociate_trial_component(TrialComponentName=component_name, TrialName=trial_name)
            try:
                # comment out to keep trial components
                sm.delete_trial_component(TrialComponentName=component_name)
            except:
                # component is associated with another trial
                continue
            # to prevent throttling
            time.sleep(.5)
        sm.delete_trial(TrialName=trial_name)
    sm.delete_experiment(ExperimentName=experiment_name)
    print(f"\nExperiment {experiment_name} deleted")
```

**Call cleanup\_boto3**

```
# Use experiment name not display name
experiment_name = "experiment-name"
cleanup_boto3(experiment_name)
```

## Clean Up Using the Experiments SDK<a name="experiments-cleanup-smsdk"></a>

**To clean up using the Experiments SDK**

```
import sys
!{sys.executable} -m pip install sagemaker-experiments
```

```
import time

from smexperiments.experiment import Experiment
from smexperiments.trial import Trial
from smexperiments.trial_component import TrialComponent
```

**Define cleanup\_sme\_sdk**

```
def cleanup_sme_sdk(experiment):
    for trial_summary in experiment.list_trials():
        trial = Trial.load(trial_name=trial_summary.trial_name)
        for trial_component_summary in trial.list_trial_components():
            tc = TrialComponent.load(
                trial_component_name=trial_component_summary.trial_component_name)
            trial.remove_trial_component(tc)
            try:
                # comment out to keep trial components
                tc.delete()
            except:
                # tc is associated with another trial
                continue
            # to prevent throttling
            time.sleep(.5)
        trial.delete()
        experiment_name = experiment.experiment_name
    experiment.delete()
    print(f"\nExperiment {experiment_name} deleted")
```

**Call cleanup\_sme\_sdk**

```
experiment_to_cleanup = Experiment.load(
    # Use experiment name not display name
    experiment_name="experiment-name")

cleanup_sme_sdk(experiment_to_cleanup)
```