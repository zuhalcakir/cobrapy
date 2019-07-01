# cobrapy
Cobrapy Tutorial Review


from __future__ import print_function
from cobra import Model, Reaction, Metabolite
# Best practise: SBML compliant IDs
mymodel = Model('example_model')

myreaction = Reaction('3OAS140', '3 oxoacyl acyl carrier protein synthase n C140 ', 'Cell Envelope Biosynthesis', 0., 1000.)
'''myreaction.name = '3 oxoacyl acyl carrier protein synthase n C140 '
myreaction.subsystem = 'Cell Envelope Biosynthesis'
myreaction.lower_bound = 0.  # This is the default
myreaction.upper_bound = 1000.  # This is the default
'''
ACP_c = Metabolite(
    'ACP_c',
    formula='C11H21N2O7PRS',
    name='acyl-carrier-protein',
    compartment='c')
omrsACP_c = Metabolite(
    '3omrsACP_c',
    formula='C25H45N2O9PRS',
    name='3-Oxotetradecanoyl-acyl-carrier-protein',
    compartment='c')
co2_c = Metabolite('co2_c', formula='CO2', name='CO2', compartment='c')
malACP_c = Metabolite(
    'malACP_c',
    formula='C14H22N2O10PRS',
    name='Malonyl-acyl-carrier-protein',
    compartment='c')
h_c = Metabolite('h_c', formula='H', name='H', compartment='c')
ddcaACP_c = Metabolite(
    'ddcaACP_c',
    formula='C23H43N2O8PRS',
    name='Dodecanoyl-ACP-n-C120ACP',
    compartment='c')

myreaction.add_metabolites({
    malACP_c: -1.0,
    h_c: -1.0,
    ddcaACP_c: -1.0,
    co2_c: 1.0,
    ACP_c: 1.0,
    omrsACP_c: 1.0
})

myreaction.reaction  # This gives a string representation of the reaction

myreaction.gene_reaction_rule = '( STM2378 or STM1197 )'
myreaction.genes

print('%i reactions initially' % len(mymodel.reactions))
print('%i metabolites initially' % len(mymodel.metabolites))
print('%i genes initially' % len(mymodel.genes))

mymodel.add_reactions([myreaction])

# Now there are things in the model
print('%i reaction' % len(mymodel.reactions))
print('%i metabolites' % len(mymodel.metabolites))
print('%i genes' % len(mymodel.genes))

# Iterate through the the objects in the model

print("\nReactions")
print("---------")
for x in mymodel.reactions:
    print("%s : %s" % (x.id, x.reaction))

print("")
print("Metabolites")
print("-----------")
for x in mymodel.metabolites:
    print('%9s : %s' % (x.id, x.formula))

print("")
print("Genes")
print("-----")
for x in mymodel.genes:
    associated_ids = (i.id for i in x.reactions)
    print("%s is associated with reactions: %s" % (x.id, "{" + ", ".join(associated_ids) + "}"))

mymodel.objective = '3OAS140'
print(mymodel.objective.expression)
print(mymodel.objective.direction)
