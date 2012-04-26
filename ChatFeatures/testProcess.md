<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<process version="5.1.002">
  <context>
    <input/>
    <output/>
    <macros/>
  </context>
  <operator activated="true" class="process" compatibility="5.1.002" expanded="true" name="Process">
    <process expanded="true" height="370" width="748">
      <operator activated="true" class="retrieve" compatibility="5.1.002" expanded="true" height="60" name="Retrieve (2)" width="90" x="45" y="75">
        <parameter key="repository_entry" value="//Samples/data/Golf"/>
      </operator>
      <operator activated="true" class="set_role" compatibility="5.1.002" expanded="true" height="76" name="Set Role" width="90" x="179" y="75">
        <parameter key="name" value="Outlook"/>
        <parameter key="target_role" value="cluster"/>
        <list key="set_additional_roles"/>
      </operator>
      <operator activated="true" class="collect" compatibility="5.1.002" expanded="true" height="60" name="Collect (2)" width="90" x="179" y="210"/>
      <operator activated="true" class="remember" compatibility="5.1.002" expanded="true" height="60" name="Remember (2)" width="90" x="313" y="210">
        <parameter key="name" value="m1"/>
        <parameter key="io_object" value="Collection"/>
      </operator>
      <operator activated="true" class="loop_clusters" compatibility="5.1.002" expanded="true" height="94" name="Loop Clusters" width="90" x="313" y="75">
        <process expanded="true" height="559" width="720">
          <operator activated="true" class="x_validation" compatibility="5.1.002" expanded="true" height="112" name="Validation (2)" width="90" x="45" y="30">
            <parameter key="number_of_validations" value="3"/>
            <parameter key="sampling_type" value="shuffled sampling"/>
            <process expanded="true" height="559" width="335">
              <operator activated="true" class="k_nn" compatibility="5.1.002" expanded="true" height="76" name="k-NN (3)" width="90" x="122" y="30">
                <parameter key="k" value="9"/>
                <parameter key="weighted_vote" value="true"/>
              </operator>
              <connect from_port="training" to_op="k-NN (3)" to_port="training set"/>
              <connect from_op="k-NN (3)" from_port="model" to_port="model"/>
              <connect from_op="k-NN (3)" from_port="exampleSet" to_port="through 1"/>
              <portSpacing port="source_training" spacing="0"/>
              <portSpacing port="sink_model" spacing="0"/>
              <portSpacing port="sink_through 1" spacing="0"/>
              <portSpacing port="sink_through 2" spacing="0"/>
            </process>
            <process expanded="true" height="559" width="335">
              <operator activated="true" class="apply_model" compatibility="5.1.002" expanded="true" height="76" name="Apply Model (2)" width="90" x="45" y="30">
                <list key="application_parameters"/>
              </operator>
              <operator activated="true" class="find_threshold" compatibility="5.1.002" expanded="true" height="76" name="Find Threshold (3)" width="90" x="179" y="30">
                <parameter key="misclassification_costs_second" value="1.4"/>
              </operator>
              <operator activated="true" class="apply_threshold" compatibility="5.1.002" expanded="true" height="76" name="Apply Threshold (3)" width="90" x="45" y="210"/>
              <operator activated="true" class="performance" compatibility="5.1.002" expanded="true" height="76" name="Performance (4)" width="90" x="179" y="210"/>
              <connect from_port="model" to_op="Apply Model (2)" to_port="model"/>
              <connect from_port="test set" to_op="Apply Model (2)" to_port="unlabelled data"/>
              <connect from_op="Apply Model (2)" from_port="labelled data" to_op="Find Threshold (3)" to_port="example set"/>
              <connect from_op="Find Threshold (3)" from_port="example set" to_op="Apply Threshold (3)" to_port="example set"/>
              <connect from_op="Find Threshold (3)" from_port="threshold" to_op="Apply Threshold (3)" to_port="threshold"/>
              <connect from_op="Apply Threshold (3)" from_port="example set" to_op="Performance (4)" to_port="labelled data"/>
              <connect from_op="Performance (4)" from_port="performance" to_port="averagable 1"/>
              <portSpacing port="source_model" spacing="0"/>
              <portSpacing port="source_test set" spacing="0"/>
              <portSpacing port="source_through 1" spacing="0"/>
              <portSpacing port="source_through 2" spacing="0"/>
              <portSpacing port="sink_averagable 1" spacing="0"/>
              <portSpacing port="sink_averagable 2" spacing="0"/>
            </process>
          </operator>
          <operator activated="true" class="multiply" compatibility="5.1.002" expanded="true" height="94" name="Multiply" width="90" x="179" y="75"/>
          <operator activated="true" class="recall" compatibility="5.1.002" expanded="true" height="60" name="Recall" width="90" x="112" y="255">
            <parameter key="name" value="m1"/>
            <parameter key="io_object" value="Collection"/>
          </operator>
          <operator activated="true" class="collect" compatibility="5.1.002" expanded="true" height="94" name="Collect" width="90" x="380" y="255">
            <parameter key="unfold" value="true"/>
          </operator>
          <operator activated="true" class="remember" compatibility="5.1.002" expanded="true" height="60" name="Remember" width="90" x="514" y="255">
            <parameter key="name" value="m1"/>
            <parameter key="io_object" value="Collection"/>
          </operator>
          <connect from_port="cluster subset" to_op="Validation (2)" to_port="training"/>
          <connect from_op="Validation (2)" from_port="model" to_port="out 1"/>
          <connect from_op="Validation (2)" from_port="averagable 1" to_op="Multiply" to_port="input"/>
          <connect from_op="Multiply" from_port="output 1" to_op="Collect" to_port="input 1"/>
          <connect from_op="Multiply" from_port="output 2" to_port="out 2"/>
          <connect from_op="Recall" from_port="result" to_op="Collect" to_port="input 2"/>
          <connect from_op="Collect" from_port="collection" to_op="Remember" to_port="store"/>
          <portSpacing port="source_cluster subset" spacing="0"/>
          <portSpacing port="source_in 1" spacing="0"/>
          <portSpacing port="sink_out 1" spacing="0"/>
          <portSpacing port="sink_out 2" spacing="0"/>
          <portSpacing port="sink_out 3" spacing="0"/>
        </process>
      </operator>
      <operator activated="true" class="recall" compatibility="5.1.002" expanded="true" height="60" name="Recall (2)" width="90" x="679" y="101">
        <parameter key="name" value="m1"/>
        <parameter key="io_object" value="Collection"/>
      </operator>
      <connect from_op="Retrieve (2)" from_port="output" to_op="Set Role" to_port="example set input"/>
      <connect from_op="Set Role" from_port="example set output" to_op="Loop Clusters" to_port="example set"/>
      <connect from_op="Collect (2)" from_port="collection" to_op="Remember (2)" to_port="store"/>
      <connect from_op="Loop Clusters" from_port="out 1" to_port="result 1"/>
      <connect from_op="Loop Clusters" from_port="out 2" to_port="result 2"/>
      <connect from_op="Recall (2)" from_port="result" to_port="result 3"/>
      <portSpacing port="source_input 1" spacing="0"/>
      <portSpacing port="sink_result 1" spacing="0"/>
      <portSpacing port="sink_result 2" spacing="0"/>
      <portSpacing port="sink_result 3" spacing="0"/>
      <portSpacing port="sink_result 4" spacing="0"/>
    </process>
  </operator>
</process>