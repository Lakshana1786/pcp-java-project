import java.util.ArrayList;
import java.util.Scanner;

class Candidate {

    int id;
    String name;
    int votes;

    Candidate(int id, String name) {
        this.id = id;
        this.name = name;
        this.votes = 0;
    }

    void addVote() {
        votes++;
    }
}

class Voter {

    int voterID;
    String name;
    boolean voted;

    Voter(int voterID, String name) {
        this.voterID = voterID;
        this.name = name;
        this.voted = false;
    }
}

class Admin {

    String username = "admin";
    String password = "1234";

    boolean login(String user, String pass) {

        if (username.equals(user) && password.equals(pass)) {
            return true;
        } else {
            return false;
        }
    }
}

class VotingMachine {

    ArrayList<Candidate> candidates = new ArrayList<>();
    ArrayList<Voter> voters = new ArrayList<>();

    Scanner sc = new Scanner(System.in);


    void initializeData() {

        candidates.add(new Candidate(1, "Ravi"));
        candidates.add(new Candidate(2, "Priya"));
        candidates.add(new Candidate(3, "Karthik"));

        voters.add(new Voter(101, "Amit"));
        voters.add(new Voter(102, "Rahul"));
        voters.add(new Voter(103, "Sneha"));
    }


    void showCandidates() {

        System.out.println("\n--- Candidate List ---");
        for (Candidate c : candidates) {
            System.out.println(c.id + ". " + c.name);
        }
    }

    void castVote() {

        System.out.print("\nEnter Voter ID: ");
        int id = sc.nextInt();

        Voter currentVoter = null;

        for (Voter v : voters) {
            if (v.voterID == id) {
                currentVoter = v;
                break;
            }
        }

        if (currentVoter == null) {
            System.out.println("Invalid Voter ID!");
            return;
        }

        if (currentVoter.voted == true) {
            System.out.println("You have already voted!");
            return;
        }

        showCandidates();

        System.out.print("Select Candidate ID: ");
        int choice = sc.nextInt();

        for (Candidate c : candidates) {
            if (c.id == choice) {
                c.addVote();
                currentVoter.voted = true;
                System.out.println("Vote Cast Successfully!");
                return;
            }
        }

        System.out.println("Invalid Candidate Selection!");
    }

    void showResult() {

        System.out.println("\n--- Election Results ---");

        for (Candidate c : candidates) {
            System.out.println(c.name + " : " + c.votes + " votes");
        }
    }
}


public class DigitalVotingMachine {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        VotingMachine machine = new VotingMachine();
        Admin admin = new Admin();

        machine.initializeData();

        int choice;

        do {

            System.out.println("\n===== DIGITAL VOTING MACHINE =====");
            System.out.println("1. Cast Vote");
            System.out.println("2. Admin Login (View Result)");
            System.out.println("3. Exit");
            System.out.print("Enter Your Choice: ");

            choice = sc.nextInt();

            switch (choice) {

                case 1:
                    machine.castVote();
                    break;

                case 2:

                    System.out.print("Enter Admin Username: ");
                    String user = sc.next();

                    System.out.print("Enter Admin Password: ");
                    String pass = sc.next();

                    if (admin.login(user, pass)) {
                        machine.showResult();
                    } else {
                        System.out.println("Invalid Login Details!");
                    }
                    break;

                case 3:
                    System.out.println("Thank You! Program Ended.");
                    break;

                default:
                    System.out.println("Invalid Choice!");
            }

        } while (choice != 3);

        sc.close();
    }
}
